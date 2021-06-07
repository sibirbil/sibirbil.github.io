Instances in classes (a), (d), (f) can be downloaded from the OR-library.


Problem Class (c)

The files aa03-aa020 and bus1, bus2 correspond to the crew scheduling instances from American Airlines. 
The format of all of these 16 files is: 
number of columns (n), number of rows (m)
the cost of each column c(j), j=1,...,n
for each column j (j=1,...,n): the number of rows which is covered by column j
followed by a list of rows covered by column j
------------------

Problem Class (e)

The files rand1-rand30 correspond to the randomly generated hard cost and coverage correlated problems.
The format of all of these 30 data files is:
number of rows (m), number of columns (n)
the cost of each column c(j),j=1,...,n
for each row i (i=1,...,m): the number of columns which cover
row i followed by a list of the columns which cover row i 
------------------

Problem Class (b)

The files coor_1-coor_80 list the coordinates of the items on the Eucledian plane. 
In each file, m x-coordinates are followed by m y-coordinates. Then, we create 320
instances based on the following scheme:
Instances 1-80: Eucledian distance & alpha=2
Instances 81-160: Eucledian distance & alpha=3
Instances 161-240: Manhattan distance & alpha=2
Instances 241-320: Manhattan distance & alpha=3

Here is a C++ code snippet that shows how to read a given Euclidean type instance as a SCP.


	int m, n; 
	vector<vector<float>>primal;
	vector <float> cost;
			

	float temp,x_1, x_2, y_1, y_2;
	float epsilon=1e-10; \\double comparison epsilon
	vector <float> x_coor;
	vector <float> y_coor;
	
	n=m*(m-1); \\for value of m see the paper

	primal.clear(); \\A matrix (sparse) for the SCP
	primal.resize(m);
	cost.clear();
	cost.resize(n);

	vector <vector <float> > distance;
	distance.resize(m);
	ifstream input;

	input.open("coor_1.txt");
		
	for (unsigned int i=0; i<m; i++)
	{
		input>>temp;
		x_coor.push_back(temp);
	}
	
	for (unsigned int i=0; i<m; i++)
	{
		input>>temp;
		y_coor.push_back(temp);
	}
	input.close();
	
	for (unsigned int i=0; i<m; i++) //finds the Eucledian distance between each pair
	{
		for (int k=0; k<m; k++)
		{
			x_1=x_coor[i];
			x_2=x_coor[k];
			y_1=y_coor[i];
			y_2=y_coor[k];
			
			if (euc==1) //input of the function
			{
				temp=sqrt((x_1-x_2)*(x_1-x_2)+(y_1-y_2)*(y_1-y_2));	//for Eucledian distances
			}
			else
			{
				temp=abs(x_1-x_2)+abs(y_1-y_2);                   //for Manhattan distances
			}
				distance[i].push_back(temp);
		}
	}
		
	
	int row=0;
	for (unsigned int i=0; i<m;i++) //sets the a matrix
		
	{
		for (unsigned int k=0; k<m; k++)
		{
			if (i!=k)
			{	
				for (int l=0; l<m; l++)
				{
					if (distance[i][l]-distance[i][k]<=epsilon)
					{
						primal[l].push_back(row);
						cost[row]=(pow(distance[i][k],alpha));	 //alpha is the cost component we used 2 and 3
							
					}
				}
				row=row+1;
			}			
		}
	}



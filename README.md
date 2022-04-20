# Numo: The basics for beginners
Welcome to the absolute beginner’s guide to Numo! If you have comments or suggestions, please don’t hesitate to [reach out](mailto:eljuancar@hotmail.com)!

## Welcome to Numo

Numo (Numeric Oracle) is an open source Oracle PL/SQL library that’s used in almost every field of numerical data in Oracle PL/SQL. Numo users include everyone from beginning coders to experienced researchers doing state-of-the-art scientific and industrial research and development.

The Numo library contains multidimensional matrix and matrix data structures (you’ll find more information about this in later sections). It provides  **t_matrix**, a homogeneous n-dimensional matrix object, with methods to efficiently operate on it. Numo can be used to perform a wide variety of mathematical operations on matrixs. It adds powerful data structures to Oracle PL/SQL that guarantee efficient calculations with matrixs and matrices and it supplies an enormous library of high-level mathematical functions that operate on these matrixs and matrices.

## Installing Numo

Numo is based in NO.PCK and some Oracle data types. You need compile all of them:

 - NO.SPC 
 - NO.BDY 
 - C.TPS 
 - MATRIX_DATA_ROW.TPS 
 - MATRIX_DATA_TABLE.TPS
 - MATRIX_INFO_ROW.TPS
 - T_MATRIX.TPS

## How to import and use Numo 

To access Numo and its functions import it in your Oracle PL/SQL code like this:

	declare
		a t_matrix;
    begin
        a = no.matrix(3,3);
    end;

We shorten the imported name to  `no`  for better readability of code using Numo. This is a widely adopted convention that you should follow so that anyone working with your code can easily understand it.

## Reading the example code

If you aren’t already comfortable with reading tutorials that contain a lot of code, you might not know how to interpret a code block that looks like this:

	declare
		a t_matrix;
		a2 t_matrix;		
    begin
        a = no.matrix(3,3);
        a2 = no.shape (a, 1, 9);
    end;

    

## What’s the difference between a common Oracle data types and a Numo matrix?

Numo gives you an enormous range of fast and efficient ways of creating matrixs and manipulating numerical data inside them. While a Oracle PL/SQL table can contain different data types within a single list, all of the elements in a Numo matrix should be homogeneous. The mathematical operations that are meant to be performed on matrixs would be extremely inefficient if the matrixs weren’t homogeneous.

**Why use Numo ?**

Numo matrix are faster and more compact than Oracle PL/SQL  tables. a matrix consumes less memory and is convenient to use. Oracle PL/SQL uses much less memory to store data and it provides a mechanism of specifying the data types. This allows the code to be optimized even further.

## What is a matrix?

A matrix is a central data structure of the Numo library. a matrix is a grid of values and it contains information about the raw data, how to locate an element, and how to interpret an element. It has a grid of elements that can be indexed in various ways. 

a matrix can be indexed by a tuple of nonnegative decimals. The  `rank`  of the matrix is the number of dimensions. The  `shape`  of the matrix is a tuple of integers giving the size of the matrix along each dimension.

One way we can initialize Numo matrixs is from Numo lists, using nested lists for two- or higher-dimensional data.

For example:

	declare
		a t_matrix;
	begin
		a := no.matrix(c(1, 2, 3, 4));
		a := no.reshape(a, 2, 2);
		no.print(a);
	end;

output:

	| 1  2 |
	| 3  4 |

## How to create a basic matrix

To create a Numo matrix, you can use the function  `no.matrix()`.

All you need to do to create a simple matrix is pass a list to it. If you choose to, you can also specify the type of data in your list.  

	declare
		a t_matrix;
    begin
	    a = no.matrix(c(1, 2, 3));
    end;

You can visualize your matrix this way:

    | 1  2  3 |

_Be aware that these visualizations are meant to simplify ideas and give you a basic understanding of Numo concepts and mechanics. matrixs and matrix operations are much more complicated than are captured here!_

Besides creating a matrix from a sequence of elements, you can easily create a matrix filled with  `0`’s:

	  declare
		a t_matrix;
      begin
	      a := no.zeros(2);
	      no.print(a);
      end;      

output:

    | 0  0 |

Or a matrix filled with  `1`’s:

	  declare
		a t_matrix;
      begin
	      a := no.ones(2);
	      no.print(a);
      end;      

output:

    | 1  1 |

Or even an empty matrix! The function  `empty`  creates a matrix whose initial content is random and depends on the state of the memory. The reason to use  `empty`  over  `zeros`  (or something similar) is speed - just make sure to fill every element afterwards!

Create an empty matrix with 2 elements:

	declare
		a t_matrix;
    begin
		a := no.empty(2) ;
	 	a := no.matrix(a,c(3.14, 42 ));  -- may vary 
		no.print(a);
    end;

output:

    | 3.1  42 |

You can create a matrix with a range of elements:

	declare
		a t_matrix;
    begin
	    a := no.arange(4);
	    print(a);
    end;

output:

    | 0  1  2  3 |

And even a matrix that contains a range of evenly spaced intervals. To do this, you will specify the  **first number**,  **last number**, and the  **step size**.

	declare
		a t_matrix;
    begin
    	a := no.arange(2, 9, 2);
    	no.print(a);
    end;

output:

    | 2  4  6  8 |

You can also use  `no.linspace()`  to create a matrix with values that are spaced linearly in a specified interval:

	declare
		a t_matrix;
    begin
    	a := no.linspace(0, 10, 5);
    	no.print(a);
    end;

output:

    | 0  2.5    5  7.5   10 |

## Getting, modifing, and sorting elements

Getting an element (i,j) is simple with  `no.get()`:

      declare
        a   t_matrix;
      begin
        a := no.arange(6);
        print(no.get(a,1,6));    
      end;

output:

	5      
	
Modify an element (i,j) with  `no.assign()`:
	
	  declare
	    a   t_matrix;
	  begin
	    a := no.arange(6);
	    a := no.assign(a,1,6,99);    
	    no.print(a);    
	  end;	

output:

	| 0   1   2   3   4  99 |

Also `assign` is a procedure, you can do:

	  declare
	    a   t_matrix;
	  begin
	    a := no.arange(6);
	    no.assign(a,1,6,99);    
	    no.print(a);    
	  end;	

With the same result.

Sorting an element is simple with  `no.sort()`. 

If you start with this matrix:

	declare
		a t_matrix;
    begin
	    a :=   no.matrix( c(2, 1, 5, 3, 7, 4, 6, 8));
	    a :=   no.sort(a);
	    no.print(a);
	end;    

You can quickly sort the numbers in ascending order with:

    | 1  2  3  4  5  6  7  8 |

## How do you know the shape and size of a matrix?

`dim`  will tell you the total number of elements of the matrix. This is the  _product_  of the elements of the matrix’s shape.

`shape`  will display a tuple of integers that indicate the number of elements stored along each dimension of the matrix. If, for example, you have a matrix with 2 rows and 3 columns, the shape of your matrix is  `(2,  3)`.

For example, if you create this matrix:

	declare
	  a t_matrix;
	begin
	  a := no.matrix(2,2,c(1, 2, 3, 4));
	end;

To find the number of dimensions of the matrix, run:

	no.print(no.shape(a));

output:

	(2,2)

To find the total number of elements in the matrix, run:

	no.print(no.dim(a));

output:

	4

And to find the rows or columnd of your matrix, run:

	no.print(no.nrow(a));
	
output:

	2	

and:	

	no.print(no.ncol(a));
	
output:

	2		

## Can you reshape a matrix?

**Yes!**

Using  `reshape()`  will give a new shape to a matrix without changing the data. Just remember that when you use the reshape method, the matrix you want to produce needs to have the same number of elements as the original matrix. If you start with a matrix with 12 elements, you’ll need to make sure that your new matrix also has a total of 12 elements.

If you start with this matrix:

	declare
	  a t_matrix;
	begin
	  a :=  no.arange(6);
	  no.print(a);
	end;

output:

	| 0 1 2 3 4 5 |

You can use  `reshape()`  to reshape your matrix. For example, you can reshape this matrix to a matrix with three rows and two columns:

	declare
	  a t_matrix;
	begin
	  a :=  no.arange(6);
	  a :=  no.reshape(a,3,2);  
	  no.print(a);
	end;

output:

	| 0  1 |
	| 2  3 |
	| 4  5 |

## Basic matrix operations

Once you’ve created your matrixs, you can start to work with them. Let’s say, for example, that you’ve created two matrixs, one called “data” and one called “ones”

You can add the matrixs together with the plus sign.

	declare
	  data   t_matrix;
	  ones   t_matrix;
	  result t_matrix;
	begin
	  data   := no.matrix(c(1, 2));
	  ones   := no.ones(2);
	  result := no.sum(data, ones);
	  no.print(result);
	end;

output:

	| 2  3 |

You can, of course, do more than just addition!

	 result := no.sub(data, ones);
	 print(result);	 

output:

	| 0  1 |

division:
	
	result := no.div(data, data );
	print(result);

output:	

	| 1  1 |
	
multiply:	

	result := no.mul_cells(data, data );
	print(result);	

output: 

	| 1  4 | 

Basic operations are simple with Numo. If you want to find the sum of the elements in a matrix, you’d use  `sum()`. This works for 1D matrixs, 2D matrixs, and matrixs in higher dimensions.

	declare
	  data   t_matrix;
	begin
	  data   := no.matrix(c(1, 2, 3 ,4));
	  no.print(no.sum(data));
	end;

output:

	10



## Broadcasting

There are times when you might want to carry out an operation between a matrix and a single number (also called  _an operation between a vector and a scalar_). For example, your matrix (we’ll call it “data”) might contain information about distance in miles but you want to convert the information to kilometers. You can perform this operation with:

	  declare
	    data   t_matrix;
	    result t_matrix;
	  begin
	    data   := no.matrix(c(1, 2, 3 ,4));
	    result := no.mul_cells(data,10);
	    no.print(result);
	  end;

output:

	| 10  20  30  40 |


Numo understands that the multiplication should happen with each cell. That concept is called  **broadcasting**. Broadcasting is a mechanism that allows Numo to perform operations on matrixs of different shapes. The dimensions of your matrix must be compatible, for example, when the dimensions of both matrixs are equal or when one of them is 1. If the dimensions are not compatible, you will get a  `Error`.

## More useful matrix operations

Numo also performs aggregation functions. In addition to  `min`,  `max`, and  `sum`, you can easily run  `mean`  to get the average,  `prod`  to get the result of multiplying the elements together,  `std`  to get the standard deviation, and more.

      declare
        data   t_matrix;
        result number;
      begin
        data   := no.matrix(c(1, 2, 3 ,4));
        result := no.prod(data);
        no.print(result);
      end;

output:


    24

Standar deviation:

    result := no.std(data);
    no.print(result);

output:

    1.1180339887

mean:

    result := no.mean(data);
    no.print(result);

output:

    2.5

max:

    result := no.max(data);
    no.print(result);

output:

    4


## Generating random numbers

The use of random number generation is an important part of the configuration and evaluation of many numerical and machine learning algorithms. Whether you need to randomly initialize weights in an artificial neural network, split data into random sets, or randomly shuffle your dataset, being able to generate random numbers (actually, repeatable pseudo-random numbers) is essential.

With  `random`, you can generate random numbers from low to high (inclusive). 

You can generate a 3 x 3 matrix of random numbers between 0 and 1 with mean zero with:

	  declare
	    data   t_matrix;
	  begin
	    data   := no.random(3,3);
	    no.print(data);
	  end;

output:

	| .1  .4  .3 |
	| .5   0  .8 |
	| .4  .5  .7 |

also you can choose the maximun, mininum and number of decimals, in this case, minimun 5, maximun 10, and zero decimals:

	  declare
	    data   t_matrix;
	  begin
	    data   := no.random(3,3,5,10,0);
	    no.print(data);
	  end;
	  
output:

    | 7   5   9 |
    | 8   8   6 |
    | 7  10   6 |


## How to get unique items 

You can find the unique elements in a matrix easily with  `no.difference`.

For example, if you start with this matrix:

	  declare
	    a   t_matrix;
	    b   t_matrix;
	  begin
	    a := no.matrix(c(11, 11, 12, 13, 14, 15, 16, 17, 12, 13, 11, 14, 18, 19, 20));
	    b := no.difference(a);
	    no.print(b);
	  end;

you can use  `no.difference`  to print the unique values in your matrix:

	| 16  18  19  15  20  14  17  11  13  12 |

## Typical matrix operations: transpose, inverse and determinant 

It’s common to need to transpose your matrices. Numo matrix have the property  `t`  that allows you to transpose a matrix.

	  declare
	    a   t_matrix;
	  begin
	    a := no.arange(6);
	    a := no.reshape(a,2, 3);
	    no.print(a);
	  end;

output

	| 0  1  2 |
	| 3  4  5 |

Apply the transpose:

	a := no.t(a);
	no.print(a);

output:

	| 0  3 |
	| 1  4 |
	| 2  5 |

Same way for to have an inverse matrix, only using `inv`

	  declare
	    a   t_matrix;
	    b   t_matrix;
	  begin
	    a := no.random(3,3);
	    b := no.inv(a);
	    no.print(b);
	  end;

output:

	| 13.6    -43   46.9 |
	| -3.8   10.9   -9.9 |
	| -8.9   34.6  -39.6 |

Determinant with `det`:

	 no.print(no.det(b)); 
	 
output:

	-144.83027602674	 

Rank with `rank`:

	 no.print(no.rank(b)); 
	 
output:

	3	 	

# Proper
proper project dimensions
##########################
# PART 1: Code
##########################

##########################
# Transposing a matrix 
##########################
def MatrixTranspose(matrix):
  B = [[0 for col in range(len(matrix))] for row in range(len(matrix[0]))] 
  for i in range(len(matrix)):
    for j in range(len(matrix[0])):
      B[j][i]=matrix[i][j]
  return B 

##########################
# Scalar Vector Multiplication
##########################

def vecScalar(vec,scalar):
  row = [ ]
  for i in range(len(vec)):
      row.append(scalar*vec[i])
  return row

##########################
# Dot Product 
########################## 
def dot(vector01,vector02):
  """
  Inputs:
    vector01: vector in format of a list.
    vector02: vector in format of a list.
  Output:
    dot:
  This function 'dot' takes inputs in the format of lists, or vectors, and performs the dot product of the vectors. Dot product refers to the itemized multiplication of corresponding elements in each vector and sums the product of the itemized multiplication. 
  If there is a mistake in the formating, an error will result specifying the inputs to change in format type so that the function can opperate properly. 
  If proper inputs are given, the end result will return a integer called 'row'.
  """
  if type(vector01)==list and type(vector02)==list:
    if len(vector01)==len(vector02):
      dot=0 
      for i in range(len(vector01)):
        dot+=vector01[i]*vector02[i]
      return dot
    else:
      return  "Incorrect sizing. Make sure both inputs are a single list of the same length. Make sure inputs are not integers, matrices(list of lists), or strings."
  else:
    return  "Incorrect sizing. Make sure both inputs are a single list of the same length. Make sure inputs are not integers, matrices(list of lists), or strings."
##########################
# Vector Subtraction
########################## 
def vecSub(vector1,vector2):
  x=[]
  if len(vector1)==len(vector2):
    for i in range(len(vector1)):
      x.append(vector1[i]-vector2[i])
    return x
##########################
# Matrix Matrix Multiplication
##########################
def matmatmult(A,B):
  if type(A)==list and type(B)==list:
    if type(A[0])==list and type(B[0])==list:
      if len(A[0])==len(B):
        X = [[0 for col in range(len(B[0]))] for row in range(len(A))]
        B = MatrixTranspose(B)
        for i in range(len(A)):
          for j in range(len(B)):
            X[i][j]= dot(A[i],B[j])
        return X
      else:
        return None
    else:
      return None
  else: 
    return None
##########################
# Normalizing Matrix 
########################## 
def normilzeMatrix(matrix):
  normalize=[]
  for i in range(len(matrix)):
    length=0
    for j in range(len(matrix[0])):
      length+=abs(matrix[i][j]**2)
    norm=(length**(1/2))
    normalize.append(vecScalar(matrix[i],(1/norm)))
  return normalize

##########################
# PART 1: Question 1
##########################
def vandermondeProject(x,y):
  n=4
  m=len(x)
  if type(x)== list and type(y)==list:
    A=[]
    if type(x[0])==list:
      return 'bad input value Change into row vector'
    else:
      for j in range(n):
        row=[]
        for i in range(m):
          row.append(x[i]**j)
        A.append(row)
      A = MatrixTranspose(A)
      return A


datay=[1.102,1.099, 1.017, 1.111, 1.117, 1.152, 1.265, 1.380, 1.575, 1.857]
datax=[.55, .60, .65,.70,.75,.80,.85,.90,.95,1.00]
A = vandermondeProject(datax,datay)
print('-----------------------')
print('Question #1')
print('Vandermonde Matrix A =')
print(A)
#print(MatrixTranspose(A))
#print(vandermondeProject(datax,datay))

##########################
# PART 1: Question 2
##########################

def Qmatrix(matrix):
  V=MatrixTranspose(matrix)
  X = [[0 for col in range(len(V[0]))] for row in range(len(V))]
  for i in range(len(V)):
    if i == 0:
      X[i] = vecScalar(V[i],-1)
    elif i == 1:
      X[i]= vecScalar(vecSub(V[i],(vecScalar(X[i-1],(dot(V[i],X[i-1])/abs(dot(X[i-1],X[i-1])))))),1)
    elif i == 2:
      proj2 = vecScalar(X[i-1],(dot(V[i],X[i-1])/abs(dot(X[i-1],X[i-1]))))
      proj1 = vecScalar(X[i-2],(dot(V[i],X[i-2])/abs(dot(X[i-2],X[i-2]))))
      projs = vecSub(proj2,vecScalar(proj1,-1))
      X[i] = vecScalar(vecSub(V[i],projs),1)
    elif i == 3:
      proj3 = vecScalar(X[i-1],(dot(V[i],X[i-1])/dot(X[i-1],X[i-1])))
      proj2 = vecScalar(X[i-2],(dot(V[i],X[i-2])/dot(X[i-2],X[i-2])))
      proj1 = vecScalar(X[i-3],(dot(V[i],X[i-3])/dot(X[i-3],X[i-3])))
      projs2 = vecSub(proj2,vecScalar(proj1,-1))
      projs = vecSub(proj3,vecScalar(projs2,-1))
      X[i] = vecScalar(vecSub(V[i],projs),-1)
    elif i == 4:
      proj4 = vecScalar(X[i-1],(dot(V[i],X[i-1])/dot(X[i-1],X[i-1])))
      proj3 = vecScalar(X[i-2],(dot(V[i],X[i-2])/dot(X[i-2],X[i-2])))
      proj2 = vecScalar(X[i-3],(dot(V[i],X[i-3])/dot(X[i-3],X[i-3])))
      proj1 = vecScalar(X[i-4],(dot(V[i],X[i-4])/dot(X[i-4],X[i-4])))
      projs = vecSub(proj4,vecSub(proj3,vecSub(proj2,proj1)))
      X[i] = vecSub(V[i],projs)
    elif i == 5:
      proj5 = vecScalar(X[i-1],(dot(V[i],X[i-1])/dot(X[i-1],X[i-1])))
      proj4 = vecScalar(X[i-2],(dot(V[i],X[i-2])/dot(X[i-2],X[i-2])))
      proj3 = vecScalar(X[i-3],(dot(V[i],X[i-3])/dot(X[i-3],X[i-3])))
      proj2 = vecScalar(X[i-4],(dot(V[i],X[i-4])/dot(X[i-4],X[i-4])))
      proj1 = vecScalar(X[i-i],(dot(V[i],X[i-i])/dot(X[i-i],X[i-i])))
      projs = vecSub(proj5,vecSub(proj4,vecSub(proj3,vecSub(proj2,proj1))))
      X[i] = vecSub(V[i],projs)
    elif i == 6:
      proj6 = vecScalar(X[i-1],(dot(V[i],X[i-1])/dot(X[i-1],X[i-1])))
      proj5 = vecScalar(X[i-2],(dot(V[i],X[i-2])/dot(X[i-2],X[i-2])))
      proj4 = vecScalar(X[i-3],(dot(V[i],X[i-3])/dot(X[i-3],X[i-3])))
      proj3 = vecScalar(X[i-4],(dot(V[i],X[i-4])/dot(X[i-4],X[i-4])))
      proj2 = vecScalar(X[i-5],(dot(V[i],X[i-5])/dot(X[i-5],X[i-5])))
      proj1 = vecScalar(X[i-i],(dot(V[i],X[i-i])/dot(X[i-i],X[i-i])))
      projs = vecSub(proj6,vecSub(proj5,vecSub(proj4,vecSub(proj3,vecSub(proj2,proj1)))))
      X[i] = vecSub(V[i],projs)
    elif i == 7:
      proj7 = vecScalar(X[i-1],(dot(V[i],X[i-1])/dot(X[i-1],X[i-1])))
      proj6 = vecScalar(X[i-2],(dot(V[i],X[i-2])/dot(X[i-2],X[i-2])))
      proj5 = vecScalar(X[i-3],(dot(V[i],X[i-3])/dot(X[i-3],X[i-3])))
      proj4 = vecScalar(X[i-4],(dot(V[i],X[i-4])/dot(X[i-4],X[i-4])))
      proj3 = vecScalar(X[i-5],(dot(V[i],X[i-5])/dot(X[i-5],X[i-5])))
      proj2 = vecScalar(X[i-6],(dot(V[i],X[i-6])/dot(X[i-6],X[i-6])))
      proj1 = vecScalar(X[i-i],(dot(V[i],X[i-i])/dot(X[i-i],X[i-i])))
      projs = vecSub(proj7,vecSub(proj6,vecSub(proj5,vecSub(proj4,vecSub(proj3,vecSub(proj2,proj1))))))
      X[i] = vecSub(V[i],projs)
    elif i == 8:
      proj8 = vecScalar(X[i-1],(dot(V[i],X[i-1])/dot(X[i-1],X[i-1])))
      proj7 = vecScalar(X[i-2],(dot(V[i],X[i-2])/dot(X[i-2],X[i-2])))
      proj6 = vecScalar(X[i-3],(dot(V[i],X[i-3])/dot(X[i-3],X[i-3])))
      proj5 = vecScalar(X[i-4],(dot(V[i],X[i-4])/dot(X[i-4],X[i-4])))
      proj4 = vecScalar(X[i-5],(dot(V[i],X[i-5])/dot(X[i-5],X[i-5])))
      proj3 = vecScalar(X[i-6],(dot(V[i],X[i-6])/dot(X[i-6],X[i-6])))
      proj2 = vecScalar(X[i-7],(dot(V[i],X[i-7])/dot(X[i-7],X[i-7])))
      proj1 = vecScalar(X[i-i],(dot(V[i],X[i-i])/dot(X[i-i],X[i-i])))
      projs = vecSub(proj8,vecSub(proj7,vecSub(proj6,vecSub(proj5,vecSub(proj4,vecSub(proj3,vecSub(proj2,proj1)))))))
      X[i] = vecSub(V[i],projs)
    elif i == 9:
      proj9 = vecScalar(X[i-1],(dot(V[i],X[i-1])/dot(X[i-1],X[i-1])))
      proj8 = vecScalar(X[i-2],(dot(V[i],X[i-2])/dot(X[i-2],X[i-2])))
      proj7 = vecScalar(X[i-3],(dot(V[i],X[i-3])/dot(X[i-3],X[i-3])))
      proj6 = vecScalar(X[i-4],(dot(V[i],X[i-4])/dot(X[i-4],X[i-4])))
      proj5 = vecScalar(X[i-5],(dot(V[i],X[i-5])/dot(X[i-5],X[i-5])))
      proj4 = vecScalar(X[i-6],(dot(V[i],X[i-6])/dot(X[i-6],X[i-6])))
      proj3 = vecScalar(X[i-7],(dot(V[i],X[i-7])/dot(X[i-7],X[i-7])))
      proj2 = vecScalar(X[i-8],(dot(V[i],X[i-8])/dot(X[i-8],X[i-8])))
      proj1 = vecScalar(X[i-i],(dot(V[i],X[i-i])/dot(X[i-i],X[i-i])))
      projs = vecSub(proj8,vecSub(proj7,vecSub(proj6,vecSub(proj5,vecSub(proj4,vecSub(proj3,vecSub(proj2,proj1)))))))
      X[i] = vecSub(V[i],projs)
  #print(X)
  #Q = MatrixTranspose(X)
  Q = normilzeMatrix(X)
   
  return Q
   
Q = Qmatrix(A)
print('-----------------------')
print('Question #2')
print('Orthanormal Matrix Q=')
print(Q)

def Rmatrix(Q,A):
  Qt = MatrixTranspose(Q)
  At = MatrixTranspose(A)
  R = matmatmult(At,Qt)
  R= MatrixTranspose(R)
  return R

R=Rmatrix(Q,A) 
print('Upper Triangular Matrix R= (Rounded to the nearest 4 decimal places)')
#print(R)
Rrounded=[[0 for col in range(len(R[0]))] for row in range(len(R))]
for i in range(len(R)):
  for j in range(len(R[0])):
    Rrounded[i][j]=round(R[i][j],4)
print(Rrounded)

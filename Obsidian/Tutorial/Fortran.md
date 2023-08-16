Fortran stands for formula translation. This is only a remembering note for the syntax of the language.
### Hello World
* The program starts with _program_ keyword and ends with _end program_ keywords. Here is a hello world example: 
```fortran
program hello
  ! This is a comment line; it is ignored by the compiler
  print *, 'Hello, World!'
end program hello
```

### Variables
* When starting the program, you must remember to put _implicit none_ to make sure that the datatypes of variables are not predetermined. 
* There are 5 built-in datatypes in fortran. They are mentioned in the following code snippet.
```fortran
program variables
  implicit none

  integer :: amount
  real :: pi
  complex :: frequency
  character :: initial
  logical :: isOkay

  amount = 10
  pi = 3.1415927
  frequency = (1.0, -0.5)
  initial = 'A'
  isOkay = .false.

end program variables
```

### IO
* There are many ways to print out information in fortran, but the simplest commands are _read_ and _print_ for input and output respectively.
```fortran
program read_value
  implicit none
  integer :: age

  print *, 'Please enter your age: '
  read(*,*) age

  print *, 'Your age is: ', age

end program read_value
```

### Arrays and Strings
* Arrays in Fortran start at index 1. 
* There are two types of arrays in fortran, static and dynamic.
#### Static Arrays
* The static arrays are declared in the way mentioned below:
```fortran
program arrays
  implicit none

  ! 1D deinteger array
  integer, dimension(10) :: array1

  ! An equivalent array declaration
  integer :: array2(10)

  ! 2D real array
  real, dimension(10, 10) :: array3

  ! Custom lower and upper index bounds
  real :: array4(0:9)
  real :: array5(-5:5)

end program arrays
```
#### Array Slicing
* Fortran has many built-in array operations including slicing and declaration of elements, as is seen in the following code:
```fortran
program array_slice
  implicit none

  integer :: i
  integer :: array1(10)  ! 1D integer array of 10 elements
  integer :: array2(10, 10)  ! 2D integer array of 100 elements

  array1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]  ! Array constructor
  array1 = [(i, i = 1, 10)]  ! Implied do loop constructor
  array1(:) = 0  ! Set all elements to zero
  array1(1:5) = 1  ! Set first five elements to one
  array1(6:) = 1  ! Set all elements after five to one

  print *, array1(1:10:2)  ! Print out elements at odd indices
  print *, array2(:,1)  ! Print out the first column in a 2D array
  print *, array1(10:1:-1)  ! Print an array in reverse

end program array_slice
```
#### Dynamic Arrays
* Dynamic arrays allows to use variable sized array. The space is allocated and de-allocated. 
```fortran
program allocatable
  implicit none

  integer, allocatable :: array1(:)
  integer, allocatable :: array2(:,:)

  allocate(array1(10))
  allocate(array2(10,10))

  ! ...

  deallocate(array1)
  deallocate(array2)

end program allocatable
```
#### Strings
* Strings, similar to arrays are of two types, static and dynamic. The example of static string declaration is given below:
```fortran
program string
  implicit none

  character(len=4) :: first_name
  character(len=5) :: last_name
  character(10) :: full_name

  first_name = 'John'
  last_name = 'Smith'

  ! String concatenation
  full_name = first_name//' '//last_name

  print *, full_name

end program string
```

### Operators 
* There are operators to compare two variables or the equation. These are mentioned below:

| **Operator** | **Alternative** |                           **Description**                           |
|:--------:|:-----------:|:---------------------------------------------------------------:|
|    ==    |     .eq.    |                Tests for equality of two operands               |
|    /=    |     .ne.    |               Test for inequality of two operands               |
|    >     |     .gt.    |   Tests if left operand is strictly greater than right operand  |
|    <     |     .lt.    |    Tests if left operand is strictly less than right operand    |
|    >=    |     .ge.    | Tests if left operand is greater than or equal to right operand |
|    <=    |     .le.    |   Tests if left operand is less than or equal to right operand  |


| **Operator** |                            **Description**                           |
|:------------:|:--------------------------------------------------------------------:|
|     .and.    |             TRUE if both left and right operands are TRUE            |
|     .or.     |        TRUE if either left or right or both operands are TRUE        |
|     .not.    |                    TRUE if right operand is FALSE                    |
|     .eqv.    |     TRUE if left operand has same logical value as right operand     |
|    .neqv.    | TRUE if left operand has the opposite logical value as right operand |

### Conditional
* Conditions can be imposed on statements using _if else_ blocks. The example is given below:
```fortran
if (angle < 90.0) then
  print *, 'Angle is acute'
else if (angle < 180.0) then
  print *, 'Angle is obtuse'
else
  print *, 'Angle is reflex'
end if
```

### Loops
* There are two types of loops in Fortran, _do loop_ & _do while loop_. 
* Do loop is similar to for loop in other languages.
```fortran
integer :: i

do i = 1, 10
  print *, i
end do
```

```fortran
integer :: i

do i = 1, 10, 2
  print *, i  ! Print odd numbers
end do
```

* Do while loops take on the form of conventional while loops.
```fortran
integer :: i

i = 1
do while (i < 11)
  print *, i
  i = i + 1
end do
! Here i = 11
```

#### Parallelizable loop
* The `do concurrent` loop is used to explicitly specify that the _inside of the loop has no interdependencies_; this informs the compiler that it may use parallelization/_SIMD_ to speed up execution of the loop and conveys programmer intention more clearly. More specifically, this means that any given loop iteration does not depend on the prior execution of other loop iterations. It is also necessary that any state changes that may occur must only happen within each `do concurrent` loop. These requirements place restrictions on what can be placed within the loop body.\
* Simply replacing a `do` loop with a `do concurrent` does not guarantee parallel execution. The explanation given above does not detail all the requirements that need to be met in order to write a correct `do concurrent` loop. Compilers are also free to do as they see fit, meaning they may not optimize the loop (e.g., a small number of iterations doing a simple calculation, like the below example). In general, compiler flags are required to activate possible parallelization for `do concurrent` loops.
```fortran
real, parameter :: pi = 3.14159265
integer, parameter :: n = 10
real :: result_sin(n)
integer :: i

do concurrent (i = 1:n)  ! Careful, the syntax is slightly different
  result_sin(i) = sin(i * pi/4.)
end do

print *, result_sin
```


Most programming languages allow you to collect commonly-used code into _procedures_ that can be reused by _calling_ them from other sections of code.
Fortran has two forms of procedure:
- **Subroutine**: invoked by a `call` statement
- **Function**: invoked within an expression or assignment to which it returns a value

### Subroutines

Following is the way to declare a subroutine:
```fortran
! Print matrix A to screen
subroutine print_matrix(n,m,A)
  implicit none
  integer, intent(in) :: n
  integer, intent(in) :: m
  real, intent(in) :: A(n, m)

  integer :: i

  do i = 1, n
    print *, A(i, 1:m)
  end do

end subroutine print_matrix
```
 And then it can be called to use in the following way:
 ```fortran
 program call_sub
  implicit none

  real :: mat(10, 20)

  mat(:,:) = 0.0

  call print_matrix(10, 20, mat)

end program call_sub
```
 * Note the additional `intent` attribute when declaring the dummy arguments; this optional attribute signifies to the compiler whether the argument is ‘’read-only’’ (`intent(in)`) ‘’write-only’’ (`intent(out)`) or ‘’read-write’’ (`intent(inout)`) within the procedure. In this example, the subroutine does not modify its arguments, hence all arguments are `intent(in)`.
 * It is good practice to always specify the `intent` attribute for dummy arguments; this allows the compiler to check for unintentional errors and provides self-documentation.
 * Subroutines are declared after the _end program_ statement.

```fortran
program vols

implicit none

!Declare variables
double precision :: rad1 , rad2, vol1, vol2
print *, "Enter the two radii!"
read *, rad1 , rad2
call volume(rad1 , vol1)
call volume(rad2 , vol2)
print *, "The difference between the two volumes is :" , abs(vol1 - vol2)
end program vols

!Subroutine to calculate volume
subroutine volume(rad , vol)
!Declare variables
double precision :: rad, vol, pi
pi = 4.0 * atan(1.0)
vol = 4. / 3. * pi * rad ** 3
end subroutine volume
```

### Functions
Function can be declared in the following way:
```fortran
! L2 Norm of a vector
function vector_norm(n,vec) result(norm)
  implicit none
  integer, intent(in) :: n
  real, intent(in) :: vec(n)
  real :: norm

  norm = sqrt(sum(vec**2))

end function vector_norm
```
Function can be called in the program in the following way:
```fortran
program run_fcn
  implicit none

  real :: v(9)
  real :: vector_norm

  v(:) = 9

  print *, 'Vector norm = ', vector_norm(9,v)

end program run_fcn
```

### 
To study
- Modules
- Derived type


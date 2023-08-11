

# properties of Matrix or Vector

Matrix is an `Eigen::Matrix<>.rows() x Eigen::Matrix<>.cols()` Martix, and it has `Eigen::Matrix<>.size()` elements.
```cpp
Eigen::MatrixXd arr(10, 20);

std::cout<< "rows : " << arr.rows() << std::endl << "cols : "<< arr.cols() << std::endl;
std::cout<< "size : " << arr.size();

>>> rows : 10
>>> cols : 20
>>> size : 200
```

Vector is an `Eigen::Matrix<>.rows()` Vector, it could be row base or col base.
```cpp
Eigen::VectorXd arr(10);

std::cout<< "rows : " << arr.rows() << std::endl << "cols : "<< arr.cols() << std::endl;
std::cout<< "size : " << arr.size();

>>> rows : 10
>>> cols : 1
>>> size : 10
```
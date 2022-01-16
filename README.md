# Camera Calibration

Camera calibration involves using the linear model of the camera to solve for the following:

1. Intrinsic Matrix (M<sub>int</sub> = [K|0])
	- Calibration Matrix or Camera Matrix (K<sub>3x3</sub>  (upper triangular matrix))
2. Extrinsic Matrix (Mext = [ [R<sub>3x3</sub> t<sub>3x1</sub>], [0<sub>1x3</sub> 1] ])
	- Rotation Matrix (R<sub>3x3</sub>)
	- Translation Matrix (t<sub>3x1</sub>)
3. Projection Matrix (p = M<sub>int</sub>.M<sub>ext</sub>)
	- Is the matrix product of the intrinsic and extrinsics

## Steps

1. Find several points in the image corresponding to some points in 3D space (2D-3D mapping)
2. Solve for Projection Matrix p
3. Use QR Decomposition of p[:3,:3] to get K, R
4. Solving Kt = p[:3,3] to get Translation Matrix t

## Solving for Projection Matrix p

Solve for projection matrix p from `Ap=0` subject to the constraint 

||p||<sup>2</sup> = 1

Define loss function:

L(p, $\lambda$) = ||Ap||<sup>2</sup> - $\lambda$.(||p||<sup>2</sup> - 1)


Equate the first derivative to 0, we get

AT.A.p = $\lambda$ .p

Solve this Eigenvalue Problem as follows:
```python
values, vectors = np.linalg.eig(np.matmul(A.T, A))


# Eigen vector corresponding to the smallest eigen value of AT.A will give p
p = vectors[np.argmin(values)]

p = np.reshape(p, (3,4) )
```

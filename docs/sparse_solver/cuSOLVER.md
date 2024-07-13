# cuSOLVER

## Include Files and Macros

```c
#include <cuda_runtime.h>
#include <cusolverSp.h>
#include <cusparse.h>

#define CUDA_CALL(func)                                                        \
{                                                                              \
    cudaError_t err = (func);                                                  \
    if (err != cudaSuccess) {                                                  \
        printf("CUDA error occurred: %s (code %d)\n", cudaGetErrorString(err), err); \
        exit(err);                                                             \
    }                                                                          \
}

#define CUSOLVER_CALL(func)                                                    \
{                                                                              \
    cusolverStatus_t status = (func);                                          \
    if (status != CUSOLVER_STATUS_SUCCESS) {                                   \
        printf("cuSolver error occurred: %d\n", status);                       \
        if(status == CUSOLVER_STATUS_ALLOC_FAILED){                            \
            printf("CUSOLVER_STATUS_ALLOC_FAILED\n");                          \
        }                                                                      \
        exit(status);                                                          \
    }                                                                          \
}

#define CUSPARSE_CALL(func)                                                    \
{                                                                              \
    cusparseStatus_t status = (func);                                          \
    if (status != CUSPARSE_STATUS_SUCCESS) {                                   \
        printf("cuSparse error occurred: %d\n", status);                       \
        exit(status);                                                          \
    }                                                                          \
}
```

## Preparation

```c
printf("Create cuSolver space...\n");
CUSOLVER_CALL(cusolverSpCreate(&(solver->handle)));
CUSOLVER_CALL(cusparseCreateMatDescr(&(solver->descrA)));
CUSPARSE_CALL(cusparseSetMatType(solver->descrA, CUSPARSE_MATRIX_TYPE_GENERAL));
CUSPARSE_CALL(cusparseSetMatIndexBase(solver->descrA, CUSPARSE_INDEX_BASE_ZERO));

csrqrInfo_t info = NULL;
CUSOLVER_CALL(cusolverSpCreateCsrqrInfo(&info));
size_t size_internal, size_qr;

int singularity, rank, *p;
p = (int*)malloc(n*sizeof(int));
double min_norm;
```

## Solve

```c
CUSOLVER_CALL(cusolverSpDcsrlsqvqrHost(solver->handle,
                                  solver->m,
                                  solver->m, 
                                  solver->nnz, 
                                  solver->descrA, 
                                  solver->val, 
                                  solver->rowA, 
                                  solver->colA, 
                                  b, 
                                  solver->tol,
                                  &rank,
                                  x,
                                  p,
                                  &min_norm));
```

## Free Resources

```
CUSOLVER_CALL(cusolverSpDestroy(solver->handle));
CUSPARSE_CALL(cusparseDestroyMatDescr(solver->descrA));
```

## Compile Options

```makefile
NVCC = nvcc
NVCC_LD = -lcudart -lcusolver -lcusparse -lcublas
direct_solver:direct_solver.c
	$(NVCC) -Xcompiler -std=c99 -w $< -o $@ $(NVCC_LD)
```

# MKL dcg

## Include Files

```
#include "mkl.h"
```

## Preparation

```c
MKL_INT             rci_request, itercount, ipar[128];
double              dpar[128];
double*             tmp       = (double*)malloc(sizeof(double) * 4 * n);
double*             temp      = (double*)malloc(sizeof(double) * n);
char                matdes[3] = {'d', 'l', 'n'};
struct matrix_descr descrA;
sparse_matrix_t     csrA;
sparse_operation_t  transA = SPARSE_OPERATION_NON_TRANSPOSE;
// create sparse matrix
descrA.type = SPARSE_MATRIX_TYPE_GENERAL;

printf("Create matrix description...\n");
mkl_sparse_d_create_csr(&csrA, SPARSE_INDEX_BASE_ZERO, n, n, (int *)solver->row_ptr, (int *)(solver->row_ptr + 1), (int *)solver->col_idx, (double *)solver->val);

// init rci
dcg_init(&n, x, b, &rci_request, ipar, dpar, tmp);

// set parameters
ipar[3]  = 0;
ipar[4]  = 100000; // Maximum iterations
ipar[7]  = 0; // performs the stopping test for the maximum number of iterations
ipar[8]  = 0; // performs the residual stopping test
ipar[9]  = 1; // Request user defined stopping test
ipar[10] = 1; // 0: Run non-preconditioned version
dpar[0]  = 1e-6; // Relative tolerance

// check
dcg_check(&n, x, b, &rci_request, ipar, dpar, tmp);
if (rci_request != 0 && rci_request != -1001) {
    printf("RCI error: %d\n", rci_request);
}

// norm
double euclidean_norm;
double  b_norm = dnrm2(&n, b, &ione);
```

## Iterative Solve

```c
while (1) {
    dcg(&n, (double *)x, b, &rci_request, ipar, dpar, tmp);
    if (rci_request == 0) {
        // succeed
        printf("succeed.\n");
        break;
    } else if (rci_request == 1) {
        // Multiply matrix by tmp[0:n-1], put in tmp[n:2*n-1] 
        mkl_sparse_d_mv(transA, 1.0, csrA, descrA, tmp, 0.0, tmp + n);
    } else if (rci_request == 2) {
        mkl_sparse_d_mv(transA, 1.0, csrA, descrA, x, 0.0, temp);
        double  eone   = -1.E0;
        daxpy (&n, &eone, b, &ione, temp, &ione);
        double r_norm_2 = dnrm2 (&n, temp, &ione);
        double  loss   = r_norm_2 / b_norm;
        printf("norm: %e\n", loss);
        if (loss < 1e-6) break;
    } else if (rci_request == 3) {
        apply_preconditioner(n, invA, tmp + 2*n, tmp + 3*n);
    } else {
        printf("RCI error: %d\n", rci_request);
        break;
    }
}
```

## Compile Options

```makefile
GCC = icpc
CFLAGS = -O3 -w -m64 -lm -std=gnu99

LDLIBS += -L /usr/lib/x86_64-linux-gnu/libm.a -lm

# Use MKL_INT as 64 bit integer
# LDLIBS += -DMKL_ILP64 -m64 -Wl,--start-group ${MKLROOT}/lib/intel64/libmkl_intel_ilp64.a ${MKLROOT}/lib/intel64/libmkl_intel_thread.a ${MKLROOT}/lib/intel64/libmkl_core.a -Wl,--end-group -liomp5 -lpthread -lm -ldl

LDLIBS += -Wl,--start-group ${MKLROOT}/lib/intel64/libmkl_intel_lp64.a ${MKLROOT}/lib/intel64/libmkl_intel_thread.a ${MKLROOT}/lib/intel64/libmkl_core.a -Wl,--end-group -liomp5 -lpthread -lm -ldl

iterative_solver:iterative_solver.c
	$(GCC) $< $(LDLIBS) -o $@ $(CFLAGS)
```




## CSR(Compressed Sparse Row Format)

* row_offset

  row[i], row[i+1] indicates the range of row i's elements' index in col_idx

  row_offset has $row\_num + 1$ elements

* col_idx

  col_idx has $row[row\_num]$ elements (0-based)

* value

  number of value = number of col_idx

![Sparse Matrices · Matt Eding](https://matteding.github.io/images/csr.gif)

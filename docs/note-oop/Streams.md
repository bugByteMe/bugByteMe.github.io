* `istream` and `ostream` are **base classes** of all streams.

  We only needs to overload:

  ```cpp
  istream& operator >> (istream &in, T &t);
  ostream& operator << (ostream &in, const T &t);
  ```

  and all kinds of stream can use this.
#### Cross-Entropy

[一文搞懂熵(Entropy),交叉熵(Cross-Entropy) - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/149186719)

* Cross-entropy between real distribution (ground truth) and predicted distribution.

```python
nn.BCEWithLogitsLoss(reduction='none')    # Sigmoid + binary cross entropy loss
```

* Sigmoid

  $\mathbb R \rightarrow [0,1]$

#### Models

* Layers

  ```python
  layers = [
          torch.nn.Linear(in_features = 3, out_features=size_h),
          torch.nn.ReLU(),
          torch.nn.Linear(in_features = size_h, out_features=size_h),
          torch.nn.ReLU(),
          torch.nn.Linear(in_features = size_h, out_features=size_h),
          torch.nn.ReLU(),
  	    # ...
          torch.nn.Linear(in_features = size_h, out_features=1)
      ]
  self.main = nn.Sequential(*layers)
  ```

  input layer + hidden layers + output layers

#### Training

* Auto grad

  ```python
  loss.backward()
  ```

  ​    Loss is computed by model output, thus on the computation graph.

​	We can exploit auto grad.

* Optimize

  ```python
  optimizer.step()
  ```

  model parameters are passed as an inference to instance optimizer.

#### Testing

* No grad

  ```python
  with torch.no_grad():
      # ...
  ```

  
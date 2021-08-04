### `product`

- 可以理解为嵌套的 for-loop 循环，比如 `product(A, B)` 等于 `((x, y) for x in A for y in B)`

- 应用1⃣️

  - 可以用在二维矩阵中的 bfs，如下所示，前后两个写法是等价的⬇️

    ```python
    for i in range(M):
      for j in range(N):
        
    
    for i, j in product(range(M), range(N))
    ```

- 
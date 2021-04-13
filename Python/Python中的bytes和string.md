#### `bytes` 🆚 `string`

- 在 Python 中，`string` 的编码方式是 `utf-8`
  - `bytes` 的开头用 `b''` 表示，内部实现是 8 bit 的值，必须用 `.decode()` 的方法得到 `string`

#### 常见功能举例🌰

- `string` 转 `bytes`

  ```python
  s = "abc"             # string
  s = "abc".encode()    # bytes，encode默认编码方式是utf-8
  s = b"abc"            # bytes
  ```

- `bytes` 转 `string`

  ```python
  s = b"abc"            # bytes
  s = b"abc".decode()   # string，encode默认编码方式是utf-8
  s = str(b"")          # string
  ```

- `bytes` 类型的 `unicode`（中文）输出

  ```python
  s = '\\u4eca\\u5929\\u5929\\u6c14\\u4e0d\\u9519'    # 中文是：今天天气不错
  new_s = s.encode().decode('unicode_escape')         # 输出为：今天天气不错
  ```

#### 两者在操作上的不相容

- 两者操作上的不相容
  - 比如你无法使用 `print(b'one' + 'two')`
  - 也无法使用 `assert b'one' > 'two'`
  - `str` 和 `bytes` 的比较总是为 `False`，*比如 `b'one' == 'one'` 就是 `False`*
  - 虽然两者一般不会同时使用，但是 `print('hello %s' % b'world')` 是可以的，但是输出其实是 `hello b'world'`
- 📒总结来说就是我们一般要么都用 `str`，要么都用 `bytes`

#### ⚠️其他注意事项

- 涉及文件的操作默认都是要用的 `str`

  ```python
  with open('text.txt', 'w') as f:
      f.write(b'hello world')  # 这一句是会报错的
  
  # 改正方式可以是将 'w' -> 'wb'
  with open('text.txt', 'wb') as f:
      f.write(b'hello world')  # 现在就不会报错了
  ```

#### 💊DBES原则

- 这是我在一本书上看到的简记的原则
- DBES 原则
  - Decode Bytes，Encode Strings
  - 意思是有 `bytes` 想得到 `string` 我们需要用 `.decode()` 方法
  - 意思是有 `string` 想得到 `bytes` 我们需要用 `.encode()` 方法


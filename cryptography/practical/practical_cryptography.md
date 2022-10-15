## Chapter 1: Cryptography: More Than Secrecy
影响密码使用效果的两大因素：1）使用者可能不清楚背后原理；2）密码方法的选择依赖于具体环境。


## Chapter 2: Hashing
Hash应该满足的特性：
- Preimage resistance: $H(x)=k$，可视为一个无穷个大数的集合。难以从一个摘要中找到一个对应原像。
- Second-preimage resistance: 已有一个文件的摘要，很难找到另一个摘要相同的原像
- Collision resistance： 很难找到产生相同输出的任何两个输入

> avalanche property(雪崩效应)：对输入微小的更改，也会使得输出变化很大

SHA-256满足以上特性，但依然存在着“原像字典问题”，即攻击者可以通过已知的SHA对查询原文。对此，可以利用'salt'混合原文，再进行hash
> 可以使用os.urandom 和 base64.b64encode来生成确定的随机值

密码参数设置：
https://cryptodoneright.org/


## Chapter 3: Symmetric Encryption: Two Sides, One Key

## Chapter 4: Asymmetric Encryption: Public/Private Keys


## Chapter 5: Message Integrity, Signatures, and Certificates


## Chapter 6: Combining Asymmetric and Symmetric Algorithms

## Chapter 7: More Symmetric Crypto: Authenticated Encryption and Kerberos


## Chapter 8: TLS Communications

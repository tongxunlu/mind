# 记住登陆

## 登录

### 限制用户输入

如什么qwert，123456, password之类，就像twitter限制用户的口令一样做一个口令的黑名单。 另外，你可以限制用户口令的长度，是否有大小写，是否有数字，你可以用你的程序做一下校验。

### 千万不要明文保存用户的口令

正如如何管理自己的口令所说的一样，很多时候，用户都会用相同的ID相同的口令来登录很多网站。 所以，如果你的网站明文保存的话，那么，如果你的数据被你的不良员工流传出去那对用户是灾难性的。 所以，用户的口令一定要加密保存，最好是用不可逆的加密，如MD5或是SHA1之类的有hash算法的不可逆的加密算法。

### HTTPS传输

因为HTTP是明文协议，所以，用户名和口令在网上也是明文发送的，这个很不安全。要做到加密传输就必需使用HTTPS协议。

## 状态保持

因为HTTP是无状态的协议，也就是说，这个协议是无法记录用户访问状态的， 其每次请求都是独立的无关联的，一笔是一笔。 而我们的网站都是设计成多个页面的，所在页面跳转过程中我们需要知道用户的状态，**所以，我们每个页面都需要对用户的身份进行认证**。

### 千万不要在cookie中存放用户的密码

加密的密码都不行。因为这个密码可以被人获取并尝试离线穷举。

### 正确设计“记住密码”

* 在cookie中，保存三个东西——**用户名，登录序列，登录token**。 **用户名**：明文存放。 **登录序列**：一个被MD5散列过的随机数，仅当强制用户输入口令时更新（如：用户修改了口令）。 **登录token**：一个被MD5散列过的随机数，仅一个登录session内有效，新的登录session会更新它。
* 上述三个东西会存在服务器上，服务器的验证用户需要验证客户端cookie里的这三个事。
* a\) **登录token** 是单实例登录。意思就是一个用户只能有一个登录实例。
* b\) **登录序列** 是用来做盗用行为检测的。 如果用户的cookie被盗后，盗用者使用这个cookie访问网站时，我们的系统是以为是合法用户， 然后更新“登录token”，而真正的用户回来访问时，系统发现只有“用户名”和“登录序列”相同， 但是“登录token” 不对，这样的话，系统就知道，这个用户可能出现了被盗用的情况， 于是，系统可以清除并更改登录序
* **上述这样的设计还是会有一些问题，比如：同一用户的不同设备登录，甚至在同一个设备上使用不同的浏览器保登录**。一个设备会让另一个设备的登录token和登录序列失效，从而让其它设备和浏览器需要重新登录，并会造成cookie被盗用的假象。所以，你在服务器服还需要考虑- IP 地址，
  * a\) 如果以口令方式登录，我们无需更新服务器的“登录序列”和 “登录token”（但需要更新cookie）。因为我们认为口令只有真正的用户知道。
  * b\) 如果 **IP相同** ，那么，我们无需更新服务器的“登录序列”和 “登录token”（但需要更新cookie）。因为我们认为是同一用户有同一IP（当然，同一个局域网里也有同一IP，但我们认为这个局域网是用户可以控制的。网吧内并不推荐使用这一功能）。
  * c\) 如果 （**IP不同 && 没有用口令登录**），那么，“登录token” 就会在多个IP间发生变化（登录token在两个或多个ip间被来来回回的变换），当在一定时间内达到一定次数后，系统才会真正觉得被盗用的可能性很高，此时系统在后台清除“登录序列”和“登录token“，让Cookie失效，强制用户输入口令（或是要求用户更改口令），以保证多台设备上的cookie一致。
* **权衡Cookie的过期时间**。 如果是永不过期，会有很不错的用户体验，但是这也会让用户很快就忘了登录密码。 如果设置上过期期限，比如2周，一个月，那么可能会好一点，但是2周和一个月后，用户依然会忘了密码。 尤其是用户在一些公共电脑上，如果保存了永久cookie的话，等于泄露了帐号。 所以，对于cookie的过期时间我们还需要权衡。

### 口令探测防守

* **使用验证码**。 验证码是后台随机产生的一个短暂的验证码，这个验证码一般是一个计算机很难识别的图片。 这样就可以防止以程序的方式来尝试用户的口令。 事实证明，这是最简单也最有效的方式。 当然，总是让用户输入那些肉眼都看不清的验证码的用户体验不好，所以，可以折中一下。 比如Google，当他发现一个IP地址发出大量的搜索后，其会要求你输入验证码。 当他发现同一个IP注册了3个以上的gmail邮箱后，他需要给你发短信方式或是电话方式的验证码。
* **用户口令失败次数**。 调置口令失败的上限，如果失败过多，则把帐号锁了，需要用户以找回口令的方式来重新激活帐号。 但是，这个功能可能会被恶意人使用。 最好的方法是，增加其尝试的时间成本（以前的这篇文章说过一个增加时间成本的解密算法）。 如，两次口令尝试的间隔是5秒钟。 三次以上错误，帐号被临时锁上30秒，5次以上帐号被锁1分钟，10次以上错误帐号被锁4小时…… 但是这会导致恶意用户用脚本来攻击，所以最好再加上验证码，验证码出错次数过多不禁止登录而是禁lP。
* **系统全局防守**。 上述的防守只针对某一个别用户。 恶意者们深知这一点，所以，他们一般会动用“僵尸网络”轮着尝试一堆用户的口令， 所以上述的那种方法可能还不够好。 我们需要在系统全局域上监控所有的口令失败的次数。 当然，这个需要我们平时没有受到攻击时的数据做为支持。 比如你的系统，平均每天有5000次的口令错误的事件，那么你可以认为， 当口令错误大幅超过这个数后，而且时间相对集中，就说明有黑客攻击。 这个时候你怎么办？一般最常见使用的方法是让所有的用户输错口令后再次尝试的时间成本增加。

最后，再说一下，关于用户登录，使用第三方的 OAuth 和 OpenID 也不失为一个很不错的选择。

# 浅谈Python的super().__ init__()

在Python中，一个class的自有属性，是可以用 __init__来定义的。当然，这个属性既可以是变量，也可以是def和其他class，比如

```python
class Router:

   def __init__(self, name, mode, number):
       self.name = name
       self.mode = mode
       self.number = number
       self.l3protocol = 'arp, static, rip, eigrp, ospf, isis, bgp'

   def desc(self):
       print(f'This is {self.name}_{self.mode}_{self.number}router')
       print(f'l3func: {self.l3protocol}')

       
if __name__ == '__main__':
   cisco = Router('CISCO', 'NEXUS', '7010')
   cisco.desc()
```

运行效果

```bash
This is CISCO_NEXUS_7010 router
l3func: arp, static, rip, eigrp, ospf, isis, bgp
```

如果，sub class Switch想要继承super class Router的__init__构造方法，应当怎么办呢？

```python
class Router:

   def __init__(self, name, mode, number):
       self.name = name
       self.mode = mode
       self.number = number
       self.l3protocol = 'arp, static, rip, eigrp, ospf, isis, bgp'

   def desc(self):
       print(f'This is {self.name}_{self.mode}_{self.number}router')
       print(f'l3func: {self.l3protocol}')


class Switch(Router):

   def __init__(self, name, mode, number, zone):
       self.zone = zone
       super().__init__(name, mode, number) # 继承super class __init__属性


   def desc(self):
       print(
           f'This is {self.name}_{self.mode}_{self.number} switch. made in {self.zone}')
       print(f'New feature: {self.l3protocol}')
       
if __name__ == '__main__':
   huawei = Switch('HUAWEI', 'CE', '12808', 'USA')
   huawei.l3protocol = 'css, vxlan, evpn, mbgp, srv6'
   huawei.desc()
```

运行效果

```bash
This is HUAWEI_CE_12808 switch. made in USA
New feature: css, vxlan, evpn, mbgp, srv6
```

### **1、谁是孙子**

在Python中，一个class可以是另一个class的sub class，当然，这个sub class同样可以是其它class的super class，很绕吧？

```python
class Router: # 是Switch、Firewall的super class
 pass
class Switch(Router): # 既是Router的sub class，也是Firewall的super class
 pass
class Firewall(Switch): # Firewall是Router的孙子吗？
 pass
```

Firewall这个class是Switch的sub class，同样也是Router的sub class，如果Router和Switch，都有各自的__init__构造方法，那么Firewall是如何继承的呢？

### **2、家族传承**

如果super class拥有__init__构造方法，sub class应当怎样继承呢？

```python
class Router:
 def __init__(self, r):
   self.r = r
   --snip--
   
class Switch(Router):
 def __init__(self, r, s):
   self.s = s
   super().__init__(r) # 二代的的super().__init__
   --snip--
   
class Firewall(Switch):
 def __init__(self, r, s, f):
   self.f = f
   super().__init__(r, s) # 三代的的super().__init__
```

## **一、__init__构造方法的继承顺序**

Python sub class中的super().__init__初始化方法，用来调用super class内的__init__构造方法。

### **1、没有__init__初始化构造的sub class**

不用super().__ init__初始化的sub class，将会自动调用super class的__ init__方法。

```python
class Cisco:

   def __init__(self):
       print('cisco')


class Huawei(Cisco):

   print('huawei')


if __name__ == '__main__':
   c = Huawei()
```

运行结果（注意顺序）

```bash
huawei
cisco
```

### **2、有__ init__构造方法的sub class，不使用super().__init__()方法**

当sub class有自己的__ init__方法，但不调用super class的__ init__方法时。sub class 就不会继承super class的__ init__方法了。

```python
class Cisco:

   def __init__(self):
       print('cisco')


class Huawei(Cisco):

   def __init__(self):
       print('huawei')


if __name__ == '__main__':
   c = Huawei()
```

运行结果

```bash
huawei
```

### **3、sub class调用super().__init__方法**

sub class在使用了super().__init__构造方法时。sub class就会继承super class的__ init__方法。

```python
class Cisco:

   def __init__(self, name):
       self.name = name
       print(self.name)


class Huawei(Cisco):

   def __init__(self, name):
       super().__init__(name)
       print('huawei')


if __name__ == '__main__':
   c = Huawei('cisco')
```

运行结果(注意顺序)

```python
cisco
huawei
```

## **二、实践**

写一个路由器的类class Router，作为super class。使用__ init__来初始化4个属性，分别是厂商名（name）、类型（mode）、子型号（number）和支持的三层协议（l3protocol）

```python
class Router:

   def __init__(self, name, mode, number):
       self.name = name
       self.mode = mode
       self.number = number
       self.l3protocol = 'arp, static, rip, eigrp, ospf, isis, bgp'
```

### **1、父与子**

再写一个交换机的sub class，使用super().__ init__继承super class的初始化方法。再添加一个新的生产地属性（zone），最后，写一个描述方法，用以描述交换机厂商名（name）、类型（mode）、子型号（number）、生产地（zone）和支持新特性（l3protocol）

```python
class Router:

   def __init__(self, name, mode, number):
       self.name = name
       self.mode = mode
       self.number = number
       self.l3protocol = 'arp, static, rip, eigrp, ospf, isis, bgp'
               
class Switch(Router):

   def __init__(self, name, mode, number, zone):
       self.zone = zone
       super().__init__(name, mode, number) # 注意


   def desc(self):
       print(
           f'This is {self.name}_{self.mode}_{self.number} switch. made in {self.zone}')
       print(f'New feature: {self.l3protocol}')
       

if __name__ == '__main__':
   huawei = Switch('HUAWEI', 'CE', '12808', 'China')
   huawei.l3protocol = 'css, vxlan, evpn, mbgp, srv6' # 继承关系，可以直接修改l3protocol的内容
   huawei.desc()
```

运行效果

```python
This is HUAWEI_CE_12808 switch. made in China
New feature: css, vxlan, evpn, mbgp, srv6
```

### **2、父子爷孙**

再写一个Switch的sub class，它的继承顺序是什么呢？

```python
class Router:

   def __init__(self, name, mode, number):
       self.name = name
       self.mode = mode
       self.number = number
       self.l3protocol = 'arp, static, rip, eigrp, ospf, isis, bgp'

   def desc(self):
       print(f'This is {self.name}_{self.mode}_{self.number}router')
       print(f'l3func: {self.l3protocol}')


class Switch(Router):

   def __init__(self, name, mode, number, zone):
       self.zone = zone
       super().__init__(name, mode, number)


   def desc(self):
       print(
           f'This is {self.name}_{self.mode}_{self.number} switch. made in {self.zone}')
       print(f'New feature: {self.l3protocol}')
       
class Firewall(Switch):
   
   def __init__(self, name, mode, number, zone):
       super().__init__(name, mode, number, zone)

if __name__ == '__main__':
   cisco = Router('CISCO', 'NEXUS', '7010')
   cisco.desc()
   huawei = Switch('HUAWEI', 'CE', '12808', 'China')
   huawei.l3protocol = 'css, vxlan, evpn, mbgp, srv6'
   huawei.desc()
   hillstone = Firewall('HILLSTONE', 'SG', '5060', 'China')
   hillstone.desc()
```

运行结果

```bash
This is CISCO_NEXUS_7010 router
l3func: arp, static, rip, eigrp, ospf, isis, bgp
This is HUAWEI_CE_12808 switch. made in China
New feature: css, vxlan, evpn, mbgp, srv6
This is HILLSTONE_SG_5060 switch. made in China # 注意看
New feature: arp, static, rip, eigrp, ospf, isis, bgp # 注意看
```

是不是一目了然呢？

**引出的问题：**

（1）sub class Firewall在调用desc方法时，是选Switch的desc，还是选Router的desc？

（2）huawei改变了l3protocol的属性：

```python
class Router:

   def __init__(self, name, mode, number):
     --snip--
     self.l3protocol = 'arp, static, rip, eigrp, ospf, isis, bgp'
       
       --snip--
huawei.l3protocol = 'css, vxlan, evpn, mbgp, srv6'
```

hillstone作为huawei子对象，调用时，为何不生效呢？

```bash
--snip--
This is HILLSTONE_SG_5060 switch. made in China
New feature: arp, static, rip, eigrp, ospf, isis, bgp
```

## **三、总结**

简单理解，super().__ init__()就是用来解决多重继承问题的。

直接用class name调用super class的方式，在单继承时，问题不大。但遇到多重继承时，就会涉及查找顺序、重复调用等问题了。

因此，可以把super().__ init__()得到的对象，简单理解成继承super class 方法时，所用的顺序表。
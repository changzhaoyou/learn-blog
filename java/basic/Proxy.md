## Dynamic Proxy Research

### 1. What is proxy mode 

- **Proxy mode** : Provide a proxy for an object, and the proxy object controls the reference to the origin object . 

- **Nature** : Control access to objects.

- **UML **:

  ![image-20200616160429085](F:%5C%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86%E5%8E%9F%E7%90%86.assets%5Cimage-20200616160429085.png)

  - Subject（abstract theme character）: Declare the common interface of real theme and proxy theme.
  - Proxy（proxy theme character）：Implement abstract theme interface and hold references to real theme , which can enhance the class method , parameters, return values of the abstract theme interface .
  - Real Subject（real theme character）：Implement abstract theme interface to realize real business operations，indirectly invoke operations defined by real theme through proxy theme.

### 2. What is static proxy

- 定义： 在编写代码的时候，就预先知道代理类、真实代理类，在编译期间就预先写好代码。创建一个接口，然后创建被代理的类实现该接口并且实现该接口中的抽象方法。之后再创建一个代理类，同时使其也实现这个接口。在代理类中持有一个被代理对象的引用，而后在代理类方法中调用该对象的方法。 

- 代码实现

  - 组合模式

    ```java
    /**
     * Subject(抽象主题角色)
     *
     * @author ycz
     * @version 1.0
     * @date 2020/5/23 23:21
     * @desc
     */
    public interface UserService {
        /**
         * 实现登录方法
         *
         * @param name
         */
        void login(String name);
    }
    
    /**
     * Proxy(代理主题对象)
     *
     * @author ycz
     * @version 1.0
     * @date 2020/5/23 23:29
     * @desc
     */
    public class UserProxy implements UserService {
        //真实主题角色
        private UserService userService;
    
        public UserProxy(UserService userService) {
            this.userService = userService;
        }
    
        @Override
        public void login(String name) {
            System.out.println("开始调用代理对象");
            userService.login(name);
            System.out.println("调用代理对象结束");
        }
    }
    
    /**
     * RealSubject(真实主题角色)
     *
     * @author ycz
     * @version 1.0
     * @date 2020/5/23 23:28
     * @desc
     */
    public class UserServiceImpl implements UserService {
        @Override
        public void login(String name) {
            System.out.println("用户：" + name + "，登录成功");
        }
    }
    ```

  - 继承模式

    ```java
    /**
     * Subject
     *
     * @author ycz
     * @version 1.0
     * @date 2020/5/23 23:21
     * @desc
     */
    public interface UserService {
        /**
         * implement login method
         *
         * @param name
         */
        void login(String name);
    }
    
    /**
     * RealSubject(真实主题角色)
     *
     * @author ycz
     * @version 1.0
     * @date 2020/5/23 23:28
     * @desc
     */
    public class UserServiceImpl implements UserService {
        @Override
        public void login(String name) {
            System.out.println("用户：" + name + "，登录成功");
        }
    }
    
    /**
     * Proxy(代理主题对象)
     *
     * @author ycz
     * @version 1.0
     * @date 2020/5/23 23:29
     * @desc
     */
    public class UserProxy extends UserServiceImpl {
    
        @Override
        public void login(String name) {
            System.out.println("开始调用代理对象");
            super.login(name);
            System.out.println("调用代理对象结束");
        }
    }
    ```

    

### 3. What is dynamic proxy  

- 定义：动态代理是一种在运行时动态地创建代理对象，动态地处理代理方法调用的机制。
- 与静态代理本质区别： 代理对象和代理方法生成的时间和方式不同。 

### 四、动态代理实现方式

 ![图16 字节码增强技术](https://p0.meituan.net/travelcube/12e1964581f38f04488dfc6d2f84f003110966.png) 

- Java Proxy

  - 代码示例：

    ```java
    /**
     * @author ycz
     * @version 1.0
     * @date 2020/6/16 14:01
     * @desc JDK动态代理-接口
     */
    public interface HelloService {
        /**
         * 请求方法
         *
         * @param name
         */
        void sayHello(String name);
    }
    
    /**
     * @author ycz
     * @version 1.0
     * @date 2020/6/16 14:04
     * @desc JDK动态代理-实现(目标类)
     */
    public class HelloServiceImpl implements HelloService {
    
        @Override
        public void sayHello(String name) {
            System.out.println(name + ",hello world!");
        }
    }
    
    /**
     * @author ycz
     * @version 1.0
     * @date 2020/6/16 14:04
     * @desc JDK动态代理-实现(代理类)
     */
    public class LogProxyHandler implements InvocationHandler {
        /**
         * 目标类
         */
        private Object target;
    
        public LogProxyHandler(Object target) {
            this.target = target;
        }
    
        @Override
        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
            System.out.println("----方法:" + method.getName() + "前置处理----");
            Object obj = method.invoke(target, args);
            System.out.println("----方法:" + method.getName() + "后置处理----");
            return obj;
        }
    }
    
    /**
     * @author ycz
     * @version 1.0
     * @date 2020/6/16 14:04
     * @desc 动态代理测试类
     */
    public class ProxyTest {
    
        public static void main(String[] args) {
            System.setProperty("sun.misc.ProxyGenerator.saveGeneratedFiles", "true");
            HelloService proxy = new HelloServiceImpl();
            HelloService helloService = (HelloService) Proxy.newProxyInstance(HelloService.class.getClassLoader(),
                    new Class[]{HelloService.class}, new LogProxyHandler(proxy));
            helloService.sayHello("张三");
        }
    }
    ```

  - 原理机制： 

    - 基于反射机制实现动态代理，JVM运行时内存生成动态字节码文件。

    - 反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为Java语言的反射机制。

    - 缺点：被代理类必须是接口，因为动态生成代理类**$Proxy0**需要实现该接口。

    - 时序图

       ![preview](https://pic2.zhimg.com/v2-19e3a6f4cc2cc0f1f9045212fe65c935_r.jpg) 

    - 源码剖析（java.lang.reflect包下）

      - InvocationHandler类

        ```java
        public interface InvocationHandler {
            /**
             * @param proxy 代理的真实代理对象com.sun.proxy.$Proxy0
             * @param Method 真实代理对象中的Method对象
             * @param args  请求真实对象中的方法所需的参数 
             */
            public Object invoke(Object proxy, Method method, Object[] args)
                throws Throwable;
        ```

      - Proxy类

        - Proxy.newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)方法

        ```Java
        /**
          * @param classloader 类加载器（被代理对象）
          * @param interfaces  一组接口（被代理对象）
          * @param InvocationHandler 代理对象实例
          */
        public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)
                throws IllegalArgumentException {
            	//1、判断代理对象实例为空抛出异常
                Objects.requireNonNull(h);
            	//2、拷贝被代理类实现的一些接口，用于后面权限方面的一些检查
                final Class<?>[] intfs = interfaces.clone();
            	//3、获取权限对象
                final SecurityManager sm = System.getSecurityManager();
                if (sm != null) {
                //4、对某些安全权限进行检查，确保我们有权限对预期的被代理类进行代理 
                    checkProxyAccess(Reflection.getCallerClass(), loader, intfs);
                }
                /*
                 * Look up or generate the designated proxy class.
                 * 5、生成指定代理类对象
                 */
                Class<?> cl = getProxyClass0(loader, intfs);
                /*
                 * Invoke its constructor with the designated invocation handler.
                 * 6、调用它指定实现invocationHandler接口的构造器
                 */
                try {
                    if (sm != null) {
                        //7、进行检查，确保真实代理类有权限
                        checkNewProxyPermission(Reflection.getCallerClass(), cl);
                    }
        			//8、获取真实代理的构造器
                    final Constructor<?> cons = cl.getConstructor(constructorParams);
                    final InvocationHandler ih = h;
                    //9、判断c1是否访问权限public,如果不是则开启特权，允许可以访问
                    if (!Modifier.isPublic(cl.getModifiers())) {
                        AccessController.doPrivileged(new PrivilegedAction<Void>() {
                            public Void run() {
                                cons.setAccessible(true);
                                return null;
                            }
                        });
                    }
                    //10、返回最终真实代理类对象
                    return cons.newInstance(new Object[]{h});
                } catch (IllegalAccessException|InstantiationException e) {
                    throw new InternalError(e.toString(), e);
                } catch (InvocationTargetException e) {
                    Throwable t = e.getCause();
                    if (t instanceof RuntimeException) {
                        throw (RuntimeException) t;
                    } else {
                        throw new InternalError(t.toString(), t);
                    }
                } catch (NoSuchMethodException e) {
                    throw new InternalError(e.toString(), e);
                }
            }
        ```

        -  代理类其实是通过getProxyClass方法来生成的： 

          ```java
             /**
               * 生成一个代理类，但是在调用本方法之前必须进行权限检查
               */
              private static Class<?> getProxyClass0(ClassLoader loader,
                                                     Class<?>... interfaces) {
                  //如果接口数量大于65535，抛出非法参数错误
                  if (interfaces.length > 65535) {
                      throw new IllegalArgumentException("interface limit exceeded");
                  }
                  // 如果在缓存中有对应的代理类，那么直接返回
                  // 否则代理类将有 ProxyClassFactory 来创建
                  return proxyClassCache.get(loader, interfaces);
              }
          ```

        - ProxyClassFactory创建代理类

          ```JAVA
             /**
               *  里面有一个根据给定ClassLoader和Interface来创建代理类的工厂函数  
               *
               */
              private static final class ProxyClassFactory
                  implements BiFunction<ClassLoader, Class<?>[], Class<?>>
              {
                  // 代理类的名字的前缀统一为“$Proxy”
                  private static final String proxyClassNamePrefix = "$Proxy";
          
                  // 每个代理类前缀后面都会跟着一个唯一的编号，如$Proxy0、$Proxy1、$Proxy2
                  private static final AtomicLong nextUniqueNumber = new AtomicLong();
          
                  @Override
                  public Class<?> apply(ClassLoader loader, Class<?>[] interfaces) {
                      Map<Class<?>, Boolean> interfaceSet = new IdentityHashMap<>(interfaces.length);
                      for (Class<?> intf : interfaces) {
                          /*
                           * 验证类加载器加载接口得到对象是否与由apply函数参数传入的对象相同
                           */
                          Class<?> interfaceClass = null;
                          try {
                              interfaceClass = Class.forName(intf.getName(), false, loader);
                          } catch (ClassNotFoundException e) {
                          }
                          if (interfaceClass != intf) {
                              throw new IllegalArgumentException(
                                  intf + " is not visible from class loader");
                          }
                          /*
                           * 验证这个Class对象是不是接口
                           */
                          if (!interfaceClass.isInterface()) {
                              throw new IllegalArgumentException(
                                  interfaceClass.getName() + " is not an interface");
                          }
                          /*
                           * 验证这个接口是否重复
                           */
                          if (interfaceSet.put(interfaceClass, Boolean.TRUE) != null) {
                              throw new IllegalArgumentException(
                                  "repeated interface: " + interfaceClass.getName());
                          }
                      }
          
                      String proxyPkg = null;     // 声明代理类所在的package
                      int accessFlags = Modifier.PUBLIC | Modifier.FINAL;
          
                      /*
                       * 记录一个非公共代理接口的包，以便在同一个包中定义代理类。同时验证所有非公共
                       * 代理接口都在同一个包中
                       */
                      for (Class<?> intf : interfaces) {
                          int flags = intf.getModifiers();
                          if (!Modifier.isPublic(flags)) {
                              accessFlags = Modifier.FINAL;
                              String name = intf.getName();
                              int n = name.lastIndexOf('.');
                              String pkg = ((n == -1) ? "" : name.substring(0, n + 1));
                              if (proxyPkg == null) {
                                  proxyPkg = pkg;
                              } else if (!pkg.equals(proxyPkg)) {
                                  throw new IllegalArgumentException(
                                      "non-public interfaces from different packages");
                              }
                          }
                      }
          
                      if (proxyPkg == null) {
                          // 如果全是公共代理接口，那么生成的代理类就在com.sun.proxy package下
                          proxyPkg = ReflectUtil.PROXY_PACKAGE + ".";
                      }
          
                      /*
                       * 为代理类生成一个name  package name + 前缀+唯一编号
                       * 如 com.sun.proxy.$Proxy0.class
                       */
                      long num = nextUniqueNumber.getAndIncrement();
                      String proxyName = proxyPkg + proxyClassNamePrefix + num;
          
                      /*
                       * 生成指定代理类的字节码文件
                       */
                      byte[] proxyClassFile = ProxyGenerator.generateProxyClass(
                          proxyName, interfaces, accessFlags);
                      try {
                          return defineClass0(loader, proxyName,
                                              proxyClassFile, 0, proxyClassFile.length);
                      } catch (ClassFormatError e) {
                          /*
                           * A ClassFormatError here means that (barring bugs in the
                           * proxy class generation code) there was some other
                           * invalid aspect of the arguments supplied to the proxy
                           * class creation (such as virtual machine limitations
                           * exceeded).
                           */
                          throw new IllegalArgumentException(e.toString());
                      }
                  }
              }
          ```

        - 生成字节码文件**$Proxy0**类

          ```java
          package com.sun.proxy;
          import com.ycz.proxy.dynamic.jdk.HelloService;
          import java.lang.reflect.InvocationHandler;
          import java.lang.reflect.Method;
          import java.lang.reflect.Proxy;
          import java.lang.reflect.UndeclaredThrowableException;
          
          public final class $Proxy0 extends Proxy implements HelloService {
              private static Method m1;
              private static Method m3;
              private static Method m2;
              private static Method m0;
          
              public $Proxy0(InvocationHandler var1) throws  {
                  super(var1);
              }
          	//super.h-->父类Proxy中InvocationHandler的实现类，也就是代理类
              public final boolean equals(Object var1) throws  {
                  try {
                      return (Boolean)super.h.invoke(this, m1, new Object[]{var1});
                  } catch (RuntimeException | Error var3) {
                      throw var3;
                  } catch (Throwable var4) {
                      throw new UndeclaredThrowableException(var4);
                  }
              }
          
              public final void sayHello(String var1) throws  {
                  try {
                      super.h.invoke(this, m3, new Object[]{var1});
                  } catch (RuntimeException | Error var3) {
                      throw var3;
                  } catch (Throwable var4) {
                      throw new UndeclaredThrowableException(var4);
                  }
              }
          
              public final String toString() throws  {
                  try {
                      return (String)super.h.invoke(this, m2, (Object[])null);
                  } catch (RuntimeException | Error var2) {
                      throw var2;
                  } catch (Throwable var3) {
                      throw new UndeclaredThrowableException(var3);
                  }
              }
          
              public final int hashCode() throws  {
                  try {
                      return (Integer)super.h.invoke(this, m0, (Object[])null);
                  } catch (RuntimeException | Error var2) {
                      throw var2;
                  } catch (Throwable var3) {
                      throw new UndeclaredThrowableException(var3);
                  }
              }
          	//通过反射获取类Method对象
              static {
                  try {
                      m1 = Class.forName("java.lang.Object").getMethod("equals", Class.forName("java.lang.Object"));
                      m3 = Class.forName("com.ycz.proxy.dynamic.jdk.HelloService").getMethod("sayHello", 	                       Class.forName("java.lang.String"));
                      m2 = Class.forName("java.lang.Object").getMethod("toString");
                      m0 = Class.forName("java.lang.Object").getMethod("hashCode");
                  } catch (NoSuchMethodException var2) {
                      throw new NoSuchMethodError(var2.getMessage());
                  } catch (ClassNotFoundException var3) {
                      throw new NoClassDefFoundError(var3.getMessage());
                  }
              }
          }
          ```

          

- Cglib

  - 什么是cglib
    - Byte Code Generation Library is high level API to generate and transform Java byte code. It is used by AOP, testing, data access frameworks to generate dynamic proxy objects and intercept field access.
    - 字节码生成库是用于生成Java字节码生成和转换高级API。可以用于AOP、测试、数据访问框架生成动态代理对接字段访问进行拦截。cglib封装了**asm**，可以在运行期动态生成新class。
  - 什么是asm
    - 
  - 
  - **注意：**final类、final方法都不能被增强。

- Javassist

- AspectJ

- Instrumentation




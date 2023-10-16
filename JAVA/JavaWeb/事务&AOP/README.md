### 1.事务管理

##### 1.1 事务

在数据库操作上我们知道什么是事务。

事务是一组操作的集合，具有四个特性：

1.原子性 ：事务是不可分割的最小单元，要么全部成功要么全部失败。

2.一致性：事务完成时，必须使所有的数据都保持一致状态。

3.隔离性： 数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。

4.持久性： 事务一旦提交回滚，它对数据的修改就是永久的。

主要操作有三步：

1.开启事务：start transaction/begin

2.提交事务：commit

3.回滚事务： rollback

##### 1.2Spring事务管理

 案例：当我们通过一个部门id对部门和本部门的员工都进行删除时，我们模拟异常发生，然后运行代码。

@Slf4j
@Service
public class DeptServiceImpl implements DeptService {
    @Autowired
    private DeptMapper deptMapper;

    @Autowired
    private EmpMapper empMapper;

 

    //根据部门id，删除部门信息及部门下的所有员工
    @Override
    public void delete(Integer id){
        //根据部门id删除部门信息
        deptMapper.deleteById(id);
        
        //模拟：异常发生
        int i = 1/0;
     
        //删除部门下的所有员工信息
        empMapper.deleteByDeptId(id);   
    }
}

通过上述代码的运行，在模拟的异常处会报错，然后往后的代码不会执行，但是前面的删除部门信息的sql已经执行过了，将数据删除成功，但是部门下的员工却没有删除，造成了数据的不一致。

所以我们就要通过事务来实现，因为一个事务中的多个业务操作，要有一致性，要么全部成功，要么全部失败。

##### 1.3添加事务

######  1.3.1Transactional注解

作用：当前方法执行之前来开启事务，方法执行完后提交事务，但是当方法出现异常后，就会进行事务的回滚操作。

@Transactional注解一般我们会在业务层来控制事务，因为在业务层当中，一个业务功能可能会包含多个数据访问的操作。在业务层来控制事务，我们就可以将多个数据访问操作控制在一个事务范围内。

@Transactional注解书写位置：

方法

当前方法交给spring进行事务管理

类

当前类中所有的方法都交由spring进行事务管理

接口

接口下所有的实现类当中所有的方法都交给spring 进行事务管理

 在删除的业务方法上加上@Transactional控制事务

@Slf4j
@Service
public class DeptServiceImpl implements DeptService {
    @Autowired
    private DeptMapper deptMapper;

    @Autowired
    private EmpMapper empMapper;


​    
    @Override
    @Transactional  //当前方法添加了事务管理
    public void delete(Integer id){
        //根据部门id删除部门信息
        deptMapper.deleteById(id);
        
        //模拟：异常发生
        int i = 1/0;
     
        //删除部门下的所有员工信息
        empMapper.deleteByDeptId(id);   
    }
}

这样就使得两句sql语句成为一个整体，当出现异常时，会进行事务回滚，保持事务一致性。

##### 1.4事务进阶

 我们知道只要加入了@Transactional注解，spring框架直接为我们自动完成添加事务等操作，我们不用管太多，但是在实际操作中，自动的肯定会有不符合我们需求的地方，我们就需要两个新属性来操作事务：

异常回滚的属性：rollbackFor

事务传播行为：propagation

###### 1.4.1 rollbackFor

我们直接添加@Transactional注解，不添加其他属性，它默认是，只有出现RuntimeException（运行期异常）才会回滚事务，而编译期异常不会进行回滚，这和现实需求肯定不符合，所以我们要使用rollbackFor属性，指定出现何种异常类型可以进行事务和回滚。

@Slf4j
@Service
public class DeptServiceImpl implements DeptService {
    @Autowired
    private DeptMapper deptMapper;

    @Autowired
    private EmpMapper empMapper;


​    
    @Override
    @Transactional(rollbackFor=Exception.class)
    public void delete(Integer id){
        //根据部门id删除部门信息
        deptMapper.deleteById(id);
        
        //模拟：异常发生
        int num = id/0;
     
        //删除部门下的所有员工信息
        empMapper.deleteByDeptId(id);   
    }
}

###### 1.4.2 propagation

###### 1.4.2.1 介绍

这个属性是用来配置事务的传播行为的，传播行为就是当一个事务方法被另一个事务方法调用时，这个事务应该如何进行事务控制。

就是指当A方法运行时，会开启一个事务，在A方法当中有调用B方法，B方法本身也有事务，那么B方法运行时，到底时加入A方法的事务，还是B方法重新创建一个新的事务。

属性值	含义
REQUIRED（主要）	(默认值) 需要事务，有则加入，无则创建新事务（主要）
REQUIRES_NEW（主要）	需要新事务i，无论有无，总是创建新事务（主要）
SUPPORTS	支持事务，有则加入，无则在无事务状态中运行
NOT_SUPPORTED	不支持事务，无事务状态下运行，如果当前存在事务，则挂起当前事务
MANDATORY	必须有事务，否则抛异常
NEVER	必须没事务，否则抛异常

###### 1.4.2.2 出现问题

 因为默认的propagation值是REQUIRED，当此方法本身有事务，并且在里一个事务中，就不会创建新的事务，直接加入到另一个事务中了，而当另一个事务出现异常，进行数据回滚时，也会将此方法也一起进行回滚，哪怕此方法没有问题。所以我们就要使用propagation属性来指定传播行为。将propagation的值赋值为REQUIRES_NEW，解决问题。

2. ##### AOP

  ##### 2.1AOP概述

   AOP英文全称：Aspect Oriented Programming（面向切面编程、面向方面编程），其实说白了，面向切面编程就是面向特定方法编程。

AOP就是面向方法编程，在不改动原始方法的基础上，针对特定的方法进行功能增强。

AOP作用：就是在程序运行期间在不修改源码的基础上对已有的方法进行增强（无侵入性；解耦）

其底层就是实现的动态代理技术，对原有的对象方法进行加强。其实AOP面向切面编程和面向对象编程一样，收拾一种编程思想，而动态代理技术是这种思想最主流的实现方式。而 Spring的AOP是Spring框架的高级技术，就是管理bean对象的过程中底层使用动态代理机制，对特定方法进行增强。

AOP优势：

减少重复代码
提高开发效率
维护方便

##### 2.2AOP快速入门

首先要导入AOP依赖

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
定义类 然后在类上添加@Aspect注解，表名这是一个切面类 再添加@Compinent 讲此类注入到spring容器中，然后创建一个返回值为object类型的参数是proceedingJoinPoint的方法，使用@Around注解 表明被AOP管理的方法 。

@Component
@Aspect //当前类为切面类
@Slf4j
public class TimeAspect {

    @Around("execution(* com.itheima.service.*.*(..))") 
    public Object recordTime(ProceedingJoinPoint pjp) throws Throwable {
        //记录方法执行开始时间
        long begin = System.currentTimeMillis();
     
        //执行原始方法
        Object result = pjp.proceed();
     
        //记录方法执行结束时间
        long end = System.currentTimeMillis();
     
        //计算方法执行耗时
        log.info(pjp.getSignature()+"执行耗时: {}毫秒",end-begin);
     
        return result;
    }
}

我们通过AOP入门程序完成了业务方法执行耗时的统计，那其实AOP的功能远不止于此，常见的应用场景如下：

记录系统的操作日志

权限控制

事务管理：我们前面所讲解的Spring事务管理，底层其实也是通过AOP来实现的，只要添加@Transactional注解之后，AOP程序自动会在原始方法运行前先来开启事务，在原始方法运行完毕之后提交或回滚事务

这些都是AOP应用的典型场景。

AOP面向切面编程的一些优势：

代码无侵入：没有修改原始的业务方法，就已经对原始的业务方法进行了功能的增强或者是功能的改变

减少了重复代码

提高开发效率

维护方便

##### 2.3AOP核心概念

1. 连接点：JoinPoint，可以被AOP控制的方法

2. 通知：Advice，指哪些重复的逻辑，也就是共性功能（最终体现为一个方法）

3. 切入点：PointCut，匹配连接点的条件，通知仅会在切入点方法执行时被应用  

4. 切面：Aspect，描述通知与切入点的对应关系（通知+切入点）  

5. 目标对象：Target，通知所应用的对象 就是被动态代理的对象

原理：Spring的AOP底层是基于动态代理技术来实现的，也就是说在程序运行的时候，会自动的基于动态代理技术为目标对象生成一个对应的代理对象。在代理对象当中就会对目标对象当中的原始方法进行功能的增强。

####   3.AOP进阶

#####  3.1通知类型

Spring中AOP的通知类型：

@Around：环绕通知，此注解标注的通知方法在目标方法前、后都被执行

@Before：前置通知，此注解标注的通知方法在目标方法前被执行

@After ：后置通知，此注解标注的通知方法在目标方法后被执行，无论是否有异常都会执行

@AfterReturning ： 返回后通知，此注解标注的通知方法在目标方法后被执行，有异常不会执行

@AfterThrowing ： 异常后通知，此注解标注的通知方法发生异常后执行

 当使用AOP处理实际问题时，就可能出现切入点多个相同的情况，那就可以抽取出来进行重复使用。Spring提供了@PointCut注解，将公共的切入点表达式抽取出来，需要用到时就直接使用。

@Slf4j
@Component
@Aspect
public class MyAspect1 {

    //切入点方法（公共的切入点表达式）
    @Pointcut("execution(* com.itheima.service.*.*(..))")
    private void pt(){
     
    }
     
    //前置通知（引用切入点）
    @Before("pt()")
    public void before(JoinPoint joinPoint){
        log.info("before ...");
     
    }
     
    //环绕通知
    @Around("pt()")
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        log.info("around before ...");
     
        //调用目标对象的原始方法执行
        Object result = proceedingJoinPoint.proceed();
        //原始方法在执行时：发生异常
        //后续代码不在执行
     
        log.info("around after ...");
        return result;
    }
     
    //后置通知
    @After("pt()")
    public void after(JoinPoint joinPoint){
        log.info("after ...");
    }
     
    //返回后通知（程序在正常执行的情况下，会执行的后置通知）
    @AfterReturning("pt()")
    public void afterReturning(JoinPoint joinPoint){
        log.info("afterReturning ...");
    }

#####  3.2通知顺序

 不同的切面类中，默认情况下通知执行顺序是与切面类名字母排序有关系的，或者在切面类上添加@Order注解，控制切面类通知的执行顺序，里面的数字大小 对于前置来说越小越先执行。

##### 3.3切点表达式

切入点表达式分为两种：

execution（）：根据方法签名来匹配，一般在有规律的方法时使用
@annotation（）：根据注解匹配，只能一个个标注。

###### 3.3.1execution

execution主要根据方法的返回值、包名、类名、方法名、方法参数等信息来匹配，语法为：execution(访问修饰符? 返回值 包名.类名.?方法名(方法参数) throws 异常?)

其中可以省略的是访问修饰符、包名.类名、throws 异常。

通配符：

*	单个独立的任意符号，可以通配任意返回值、包名、类名、方法名、任意类型的参数
..	多个连续的任意符号，可以通配任意层级的包，或任意类型个数的参数
切入点表达式示例

省略方法的修饰符号

execution(void com.itheima.service.impl.DeptServiceImpl.delete(java.lang.Integer))
使用*代替返回值类型

execution(* com.itheima.service.impl.DeptServiceImpl.delete(java.lang.Integer))
使用*代替包名（一层包使用一个*）

execution(* com.itheima.*.*.DeptServiceImpl.delete(java.lang.Integer))
使用..省略包名

execution(* com..DeptServiceImpl.delete(java.lang.Integer))    
使用*代替类名

execution(* com..*.delete(java.lang.Integer))   
使用*代替方法名

execution(* com..*.*(java.lang.Integer))   
使用 * 代替参数

execution(* com.itheima.service.impl.DeptServiceImpl.delete(*))
使用..省略参数

execution(* com..*.*(..))
根据业务需要，可以使用 且（&&）、或（||）、非（!） 来组合比较复杂的切入点表达式。

######  3.3.2@annotation

 如果要匹配没有规则的方法，将切入点组合太麻烦了，我们就可以使用annotation来描述切入点。

首先要自定义一个注解

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyLog {
}
其次再需要AOP管理的方法上添加自己创建的注解

@Slf4j
@Service
public class DeptServiceImpl implements DeptService {
    @Autowired
    private DeptMapper deptMapper;

    @Override
    @MyLog //自定义注解（表示：当前方法属于目标方法）
    public List<Dept> list() {
        List<Dept> deptList = deptMapper.list();
        //模拟异常
        //int num = 10/0;
        return deptList;
    }
     
    @Override
    @MyLog  //自定义注解（表示：当前方法属于目标方法）
    public void delete(Integer id) {
        //1. 删除部门
        deptMapper.delete(id);
    }

#####  创建切面类

@Slf4j
@Component
@Aspect
public class MyAspect6 {
    //针对list方法、delete方法进行前置通知和后置通知

    //前置通知
    @Before("@annotation(com.itheima.anno.MyLog)")
    public void before(){
        log.info("MyAspect6 -> before ...");
    }
     
    //后置通知
    @After("@annotation(com.itheima.anno.MyLog)")
    public void after(){
        log.info("MyAspect6 -> after ...");
    }
}

##### 两种切入点表达式选择

execution切入点表达式

根据我们所指定的方法的描述信息来匹配切入点方法，这种方式也是最为常用的一种方式

如果我们要匹配的切入点方法的方法名不规则，或者有一些比较特殊的需求，通过execution切入点表达式描述比较繁琐

annotation 切入点表达式

基于注解的方式来匹配切入点方法。这种方式虽然多一步操作，我们需要自定义一个注解，但是相对来比较灵活。我们需要匹配哪个方法，就在方法上加上对应的注解就可以了

##### 3.4连接点


在Spring中用JoinPoint抽象了连接点，用它可以获得方法执行时的相关信息，如目标类名、方法名、方法参数等。

对于@Around通知，获取连接点信息只能使用ProceedingJoinPoint类型

对于其他四种通知，获取连接点信息只能使用JoinPoint，它是ProceedingJoinPoint的父类型

关键是只有@Around通知，需要运行目标对象的方法，需要使用 proceed()方法。

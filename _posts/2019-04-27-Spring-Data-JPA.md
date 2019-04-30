---

title: Spring Data JPA
date: 2019-04-27 00:44:54
tags: 技术分享 

---

### 1.创建Customer类
```java
package hello;

import javax.persistence.Entity;import javax.persistence.GeneratedValue;import javax.persistence.GenerationType;import javax.persistence.Id;

@Entity//表名这是一个JPA的实体，因为缺少@Table注解，可以认为这个实体将会被映射成一个名为Customer的表
public class Customer {

    @Id//JPA会通过@ID标签识别出对象的ID
    @GeneratedValue(strategy=GenerationType.AUTO)// @GeneratedValue指出这是一个自增的ID
    private Long id;
    private String firstName;//firstName和lastName这两个属性将会被映射成相同名字的字段
    private String lastName;

    protected Customer() {}//默认的构造方法，只是为了JPA的利益而存在，你不会直接使用到它，所以它被指定为protected

    public Customer(String firstName, String lastName) {//构建Customer的实例以便保存到数据库
        this.firstName = firstName;
        this.lastName = lastName;
    }

    @Override
    public String toString() {//重写toString方法，打印数据
        return String.format(
                "Customer[id=%d, firstName='%s', lastName='%s']",
                id, firstName, lastName);
    }

}
```
### 2.新建一个简单的查询
Spring Data JPA专注于使用JPA保存数据到关系型数据库中，它最引人注目的地方是在运行时通过repository 接口有自动创建repository 的实现的能力。
为了展示它是如何工作的，建立了一个repository接口，work with Customer实体。
```java
package hello;

import java.util.List;

import org.springframework.data.repository.CrudRepository;

public interface CustomerRepository extends CrudRepository<Customer, Long> {

    List<Customer> findByLastName(String lastName);}
```
CustomerRepository 继承了 CrudRepository 接口. 这个实体和ID的类型是Customer和Long。通过继承CrudRepository接口，
CustomerRepository 继承了一些方法，包括保存，删除和查找Customer实体。
JPA也允许你定义其他的方法，例如CustomerRepository中的findByLastName方法。
在传统的Java程序中，你要写一个实现类去实现CustomerRepository接口，但是Spring Data JPA强大的地方就是你不必写CustomerRepository接口的实现类，Spring Data JPA 在你运行应用的时候自动创建了实现类。
### 3.创建一个Application
```java
package hello;

import org.slf4j.Logger;import org.slf4j.LoggerFactory;

import org.springframework.boot.CommandLineRunner;import org.springframework.boot.SpringApplication;import org.springframework.boot.autoconfigure.SpringBootApplication;import org.springframework.context.annotation.Bean;

@SpringBootApplicationpublic 
class Application {

        private static final Logger log = LoggerFactory.getLogger(Application.class);

        public static void main(String[] args) {
                SpringApplication.run(Application.class);
        }

        @Bean
        public CommandLineRunner demo(CustomerRepository repository) {
                return (args) -> {
                        // save a couple of customers
                        repository.save(new Customer("Jack", "Bauer"));
                        repository.save(new Customer("Chloe", "O'Brian"));
                        repository.save(new Customer("Kim", "Bauer"));
                        repository.save(new Customer("David", "Palmer"));
                        repository.save(new Customer("Michelle", "Dessler"));

                        // fetch all customers
                        log.info("Customers found with findAll():");
                        log.info("-------------------------------");
                        for (Customer customer : repository.findAll()) {
                                log.info(customer.toString());
                        }
                        log.info("");

                        // fetch an individual customer by ID
                        repository.findById(1L)
                                .ifPresent(customer -> {
                                        log.info("Customer found with findById(1L):");
                                        log.info("--------------------------------");
                                        log.info(customer.toString());
                                        log.info("");
                                });

                        // fetch customers by last name
                        log.info("Customer found with findByLastName('Bauer'):");
                        log.info("--------------------------------------------");
                        repository.findByLastName("Bauer").forEach(bauer -> {
                                log.info(bauer.toString());
                        });
                        // for (Customer bauer : repository.findByLastName("Bauer")) {
                        //      log.info(bauer.toString());
                        // }
                        log.info("");
                };
        }

}
```
SpringBootApplication 这个注解相当于添加了以下几个注解：

* @Configuration 这个类对于application context来说是一个源bean
* @EnableAutoConfiguration 告诉Spring Boot开始根据配置文件添加beans。
* 通常来讲，对于一个SpringMVC程序你应该添加@EnableWebMvc注解，但是SpringBoot当它在类路径中看到
  **spring-webmvc** 时就自动添加了，这会标记这个程序是一个web程序并且激活关键的行为， 比如建立一个DispatcherServlet。
* @ComponentScan 告诉Spring在hello包中去寻找其它的组件，配置，service，允许它去寻找controller
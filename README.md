# SpringNotes
## Making notes during Spring learning &amp; practice

#### 1. Simple application xml-based config: 
applicationContext.xml
```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans  xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:context="http://www.springframework.org/schema/context"
            xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
    
        <context:property-placeholder location="classpath:sport.properties"/>
        <bean id="myFortuneService"
              class="com.luv2code.springdemo.fortune.HappyFortuneService" />
        <bean id="myCoach"
              class="com.luv2code.springdemo.coach.TrackCoach"
              init-method="doStartupDeals"
              destroy-method="doCleanupDeals"
              scope="prototype">
            <constructor-arg ref="myFortuneService"/>
        </bean>
        <bean id="cricketCoach" class="com.luv2code.springdemo.coach.CricketCoach">
            <property name="fortuneService" ref="myFortuneService"/>
            <property name="email" value="${foo.email}"/>
            <property name="team" value="${foo.team}"/>
        </bean>
    </beans>
```
 sport.properties
 
    foo.email=bestcoach@google.com
    foo.team=theBestTeamEver
    
TrackCoach.java
```java
    package com.luv2code.springdemo.coach;
    import com.luv2code.springdemo.fortune.FortuneService;

    public class TrackCoach implements Coach {

        private final FortuneService fortuneService;

        public TrackCoach(FortuneService fortuneService) {
            this.fortuneService = fortuneService;
        }

        @Override
        public String getDailyWorkout() {
            return "Run hard 18k meters";
        }

        @Override
        public String getDailyFortune() {
            return fortuneService.getFortune();
        }

        public void doStartupDeals() {
            System.out.println("Track coach: inside startup method");
        }

        public void doCleanupDeals() {
            System.out.println("Track coach: inside cleanup method");
        }
}
```

CricketCoach.java
```java
    package com.luv2code.springdemo.coach;
    import com.luv2code.springdemo.fortune.FortuneService;

    public class CricketCoach implements Coach {
        private FortuneService fortuneService;
        private String email;
        private String team;
    
        public void setFortuneService(FortuneService fortuneService) {
            this.fortuneService = fortuneService;
        }
    
        public void setEmail(String email) {
            this.email = email;
        }
    
        public void setTeam(String team) {
            this.team = team;
        }
    
        public String getEmail() {
            return email;
        }
    
        public String getTeam() {
            return team;
        }
    
        public CricketCoach() {}
    
        @Override
        public String getDailyWorkout() {
            return "OLOLO";
        }
    
        @Override
        public String getDailyFortune() {
            return fortuneService.getFortune();
        }
}
```
    
App.java
 ```java
     package com.luv2code.springdemo;
     import com.luv2code.springdemo.coach.Coach;
     import org.springframework.context.support.ClassPathXmlApplicationContext;
 
     public class App {
         public static void main(String[] args) {
             ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(
                     "applicationContext.xml");
             Coach coach = context.getBean("myCoach", Coach.class);
             System.out.println(coach.getDailyFortune());
             context.close();
         }
     }
```
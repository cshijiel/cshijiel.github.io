title: "SpringBoot中读取YAML"
date: 2015-04-07 00:20:36
tags: 
- Spring
- YAML
categories: 知识记录
---
1. 在使用框架的时候，能用框架解决的使用框架解决。实在不能用框架解决的再考虑使用其他办法。


2. 使用Spring注解填充信息的对象不可以自己实例化，应该使用注解（@Resource或这 @Autowired  ）实例化。

----

读取YAML步骤：


1. 在`application.yml`文件中，加入需要注入的信息：

        my:
            servers:
                - dev.bar.com
                - foo.bar.com

2. 在需要注解信息的Java文件中进行以下注解：

        @Component("config")
        @ConfigurationProperties(prefix = "my")
        public class Config {
        	//@Value("my.servers") 
                //这一句完全不需要，无效。需要把该填充的数据放到`application.yml`即可！
        	private List<String> servers = new ArrayList<String>();
        	public List<String> getServers() {
        		return this.servers;
        	}
        }

3. 在需要引用的地方使用Spring注入（`@Resource`或这 `@Autowired`）实例化。

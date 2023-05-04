<plugin>

   <groupId>org.springframework.boot</groupId>

   <artifactId>spring-boot-maven-plugin</artifactId>

   <version>2.4.2</version>

   <executions>

      <execution>

         <goals>

            <goal>repackage</goal>

            <goal>build-info</goal>

         </goals>

      </execution>

   </executions>

   <configuration>

      <fork>true</fork>

   </configuration>

</plugin>


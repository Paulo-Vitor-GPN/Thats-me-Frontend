package br.com.santander.yz.config;

import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.Properties;
import javax.sql.DataSource;
import lombok.Getter;
import lombok.Setter;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;

@Getter
@Setter
@Configuration
@Profile({"local", "test"})
@ConfigurationProperties(prefix = "database.h2")
public class DatabaseH2Config {

  private String url;
  private String driverClassName;
  private String username;
  private String password;
  private String poolName;
  private Integer maxPoolSize;
  private Integer minPoolSize;
  private Integer maxLifetime;
  private Integer validationTimeout;
  private Integer connectionTimeout;
  private Integer idleTimeout;
  private Integer leakDetectionThreshold;

  /**
   * Creates a DataSource Bean to connect to the database.
   *
   * @return Connection DataSource.
   */
  @Bean
  public DataSource dataSource() throws IOException {
    final Properties properties = new Properties();
    properties.put("DATABASE", "H2");
    properties.put("SHOULD_MOCK_DATA", true);

    HikariConfig hikariConfig = new HikariConfig();
    hikariConfig.setDriverClassName(driverClassName);
    hikariConfig.setJdbcUrl(url);
    hikariConfig.setUsername(extractSecretValue(username));
    hikariConfig.setPassword(extractSecretValue(password));
    hikariConfig.setPoolName(poolName);
    hikariConfig.setMinimumIdle(minPoolSize);
    hikariConfig.setMaximumPoolSize(maxPoolSize);
    hikariConfig.setMaxLifetime(maxLifetime);
    hikariConfig.setValidationTimeout(validationTimeout);
    hikariConfig.setConnectionTimeout(connectionTimeout);
    hikariConfig.setIdleTimeout(idleTimeout);
    hikariConfig.setLeakDetectionThreshold(leakDetectionThreshold);
    hikariConfig.setDataSourceProperties(properties);
    return new HikariDataSource(hikariConfig);
  }

  /**
   * Tests if the provided secret is an existing file. If it's a file, the contents are read and
   * assumed as the password. Otherwise, we assume the password was passed in plain text.
   *
   * @param secret Path to a file containg the password or the password itself.
   * @return The password to be used for connection.
   */
  private static String extractSecretValue(String secret) throws IOException {
    Path secretPath = Path.of(secret);
    return Files.readString(secretPath);
  }
}

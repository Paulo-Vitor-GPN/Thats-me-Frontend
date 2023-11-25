package br.com.santander.yz.config;

import br.com.santander.yz.model.*;
import br.com.santander.yz.model.enums.ExecutionTypeEnum;
import br.com.santander.yz.model.enums.StimulusTypeEnum;
import br.com.santander.yz.repository.StimulusParameterRepository;
import br.com.santander.yz.repository.StimulusScheduleRepository;
import com.zaxxer.hikari.HikariDataSource;
import java.time.LocalDateTime;
import java.util.HashSet;
import java.util.Random;
import java.util.Set;
import java.util.UUID;
import lombok.AllArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.boot.CommandLineRunner;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;

@AllArgsConstructor
@Configuration
@Profile("sandbox")
@Slf4j
public class H2MockConfig implements CommandLineRunner {

  private StimulusParameterRepository parameterRepository;
  private StimulusScheduleRepository scheduleRepository;

  private HikariDataSource dataSource;

  @Override
  public void run(String... args) throws Exception {

    log.info("Environment sandbox ativo.");

    if (!"H2".equals(dataSource.getDataSourceProperties().get("DATABASE"))
        && Boolean.TRUE.equals(dataSource.getDataSourceProperties().get("SHOULD_MOCK_DATA"))) {
      return;
    }

    log.info("Mockando dados em base de dados temporária.");

    for (int i = 0; i < 999; i++) {
      StimulusParameter parameter = generateStimulusParameter();

      if (parameterRepository.findById(parameter.getStimulusId()).isPresent()) {
        continue;
      }

      parameterRepository.save(parameter);
      scheduleRepository.saveAll(parameter.getSchedules());
    }
  }

  private StimulusParameter generateStimulusParameter() {
    StimulusParameter parameter =
        new StimulusParameter(
            generateStimulusId(),
            LocalDateTime.now(),
            generateTitle(),
            generateStimulusType(),
            generateExecutionType(),
            generateEnabled(),
            null);
    Set<StimulusSchedule> schedules =
        generateSchedules(parameter.getExecutionTypeEnum(), parameter);
    parameter.setSchedules(schedules);
    return parameter;
  }

  private String generateStimulusId() {
    return "YZBATCH" + new Random().nextInt(999);
  }

  private String generateTitle() {
    return UUID.randomUUID().toString();
  }

  private ExecutionTypeEnum generateExecutionType() {
    int r = new Random().nextInt(4);
    switch (r) {
      case 0:
        return ExecutionTypeEnum.DAILY;
      case 1:
        return ExecutionTypeEnum.MONTHLY;
      case 2:
        return ExecutionTypeEnum.YEARLY;
      case 3:
        return ExecutionTypeEnum.EVENTUALLY;
    }
    return ExecutionTypeEnum.DAILY;
  }

  private StimulusTypeEnum generateStimulusType() {
    int r = new Random().nextInt(4);
    switch (r) {
      case 0:
        return StimulusTypeEnum.EMAIL;
      case 1:
        return StimulusTypeEnum.PUSH;
      case 2:
        return StimulusTypeEnum.SMS;
      case 3:
        return StimulusTypeEnum.FALLBACK;
    }
    return StimulusTypeEnum.EMAIL;
  }

  private Boolean generateEnabled() {
    int r = new Random().nextInt(2);
    return r == 0 ? true : false;
  }

  private Set<StimulusSchedule> generateSchedules(
      ExecutionTypeEnum executionTypeEnum, StimulusParameter stimulusParameter) {
    int r = new Random().nextInt(5);
    r = r == 0 ? r++ : r;

    Set<StimulusSchedule> schedules = new HashSet<>();

    switch (executionTypeEnum) {
      case DAILY:
        for (int i = 0; i <= r; i++) {
          schedules.add(
              new StimulusSchedule(
                  null, stimulusParameter, generateRandomRange(0, 23), null, null));
        }
        break;
      case MONTHLY:
        for (int i = 0; i <= r; i++) {
          schedules.add(
              new StimulusSchedule(
                  null,
                  stimulusParameter,
                  generateRandomRange(0, 23),
                  generateRandomRange(1, 31),
                  null));
        }
        break;
      case YEARLY:
        for (int i = 0; i <= r; i++) {
          schedules.add(
              new StimulusSchedule(
                  null,
                  stimulusParameter,
                  generateRandomRange(0, 23),
                  generateRandomRange(1, 31),
                  generateRandomRange(1, 12)));
        }
        break;
      case EVENTUALLY:
        return new HashSet<>();
    }
    return schedules;
  }

  private Integer generateRandomRange(Integer min, Integer max) {
    return (int) Math.floor(Math.random() * (max - min + 1) + min);
  }
}

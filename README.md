package br.com.santander.yzcaml.clientintegration.model.dto;

import br.com.santander.yzcaml.clientintegration.model.enums.ExecutionType;
import br.com.santander.yzcaml.clientintegration.model.enums.StimulusType;
import com.fasterxml.jackson.annotation.JsonFormat;
import lombok.*;

import java.time.LocalDateTime;
import java.util.Set;

@AllArgsConstructor
@NoArgsConstructor
@Getter
@Setter
@ToString

public class StimulusDTO {

    private String stimulusId;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd'T'HH:mm:ss")
    public LocalDateTime createdAt;
    private String title;
    private ExecutionType executionType;
    private StimulusType stimulusType;
    private Boolean enabled;
    private Set<StimulusScheduleDTO> schedules;

}


package br.com.santander.yzcaml.clientintegration.model.enums;

public enum ExecutionType {
    DAILY,
    MONTHLY,
    YEARLY,
    EVENTUALLY
}


package br.com.santander.yzcaml.clientintegration.model.enums;

public enum StimulusType {
    EMAIL,
    PUSH,
    SMS,
    FALLBACK,
    WHATSAPP
}


package br.com.santander.yzcaml.clientintegration.model.dto;

import lombok.*;

@NoArgsConstructor
@AllArgsConstructor
@Getter
@Setter
@ToString

public class StimulusScheduleDTO {

    private Integer hour;
    private Integer day;
    private Integer month;

}

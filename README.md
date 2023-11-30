
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

import br.com.santander.yzcaml.clientintegration.model.enums.ExecutionType;
import br.com.santander.yzcaml.clientintegration.model.enums.StimulusType;
import com.fasterxml.jackson.annotation.JsonFormat;
import com.fasterxml.jackson.databind.annotation.JsonDeserialize;
import com.fasterxml.jackson.databind.annotation.JsonSerialize;
import com.fasterxml.jackson.datatype.jsr310.deser.LocalDateDeserializer;
import com.fasterxml.jackson.datatype.jsr310.ser.LocalDateSerializer;
import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

import java.time.LocalDate;
import java.util.Set;

@Getter
@Setter
@ToString
public class CreateStimuliDTO {

    private String stimulusId;
    @JsonDeserialize(using = LocalDateDeserializer.class)
    @JsonSerialize(using = LocalDateSerializer.class)
    @JsonFormat(pattern = "yyyy-MM-dd")
    private LocalDate createdAt;
    private String title;
    private ExecutionType executionType;
    private StimulusType stimulusType;
    private Boolean enabled;
    private String msgId;
    private Set<StimulusScheduleDTO> schedules;

    public static boolean validFields(CreateStimuliDTO createStimuliDTO){

        return true;
    }
}

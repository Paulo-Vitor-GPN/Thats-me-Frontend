import org.springframework.util.StringUtils;

public class CreateStimuliDTOValidator {

    public static boolean isValid(CreateStimuliDTO createStimuliDTO) {
        return isValidStimulusId(createStimuliDTO.getStimulusId()) &&
               isValidCreatedAt(createStimuliDTO.getCreatedAt()) &&
               isValidTitle(createStimuliDTO.getTitle()) &&
               isValidExecutionType(createStimuliDTO.getExecutionType()) &&
               isValidStimulusType(createStimuliDTO.getStimulusType()) &&
               isValidEnabled(createStimuliDTO.getEnabled()) &&
               isValidMsgId(createStimuliDTO.getMsgId()) &&
               isValidSchedules(createStimuliDTO.getSchedules());
    }

    private static boolean isValidStimulusId(String stimulusId) {
        return !StringUtils.isEmpty(stimulusId);
    }

    private static boolean isValidCreatedAt(LocalDate createdAt) {
        return createdAt != null;
    }

    private static boolean isValidTitle(String title) {
        return !StringUtils.isEmpty(title);
    }

    private static boolean isValidExecutionType(ExecutionType executionType) {
        return executionType != null;
    }

    private static boolean isValidStimulusType(StimulusType stimulusType) {
        return stimulusType != null;
    }

    private static boolean isValidEnabled(Boolean enabled) {
        return enabled != null;
    }

    private static boolean isValidMsgId(String msgId) {
        return !StringUtils.isEmpty(msgId);
    }

    private static boolean isValidSchedules(Set<StimulusScheduleDTO> schedules) {
        return schedules != null && schedules.stream().allMatch(CreateStimuliDTOValidator::isValidSchedule);
    }

    private static boolean isValidSchedule(StimulusScheduleDTO schedule) {
        return schedule != null &&
               schedule.getHour() != null &&
               schedule.getDay() != null &&
               schedule.getMonth() != null;
    }
}

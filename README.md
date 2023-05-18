import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;

public class JsonValidator {
    private ObjectMapper objectMapper;

    public JsonValidator() {
        objectMapper = new ObjectMapper();
    }

    public boolean hasNullOrBlankFields(String jsonString) {
        try {
            JsonNode jsonNode = objectMapper.readTree(jsonString);
            return hasNullOrBlankFields(jsonNode);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    private boolean hasNullOrBlankFields(JsonNode jsonNode) {
        if (jsonNode.isObject()) {
            Iterator<Map.Entry<String, JsonNode>> fields = jsonNode.fields();
            while (fields.hasNext()) {
                Map.Entry<String, JsonNode> field = fields.next();
                JsonNode fieldValue = field.getValue();
                if (fieldValue == null || fieldValue.isNull() || fieldValue.isTextual() && fieldValue.asText().isEmpty()) {
                    return true;
                }
                if (fieldValue.isContainerNode() && hasNullOrBlankFields(fieldValue)) {
                    return true;
                }
            }
        } else if (jsonNode.isArray()) {
            for (JsonNode arrayElement : jsonNode) {
                if (arrayElement.isObject() && hasNullOrBlankFields(arrayElement)) {
                    return true;
                }
            }
        }
        return false;
    }
}

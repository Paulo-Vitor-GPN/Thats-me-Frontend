import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;

import java.util.Iterator;
import java.util.Map;

public class JsonValidator {
    private ObjectMapper objectMapper;

    public JsonValidator() {
        objectMapper = new ObjectMapper();
    }

    public boolean areAllFieldsEmpty(String jsonString) {
        try {
            JsonNode jsonNode = objectMapper.readTree(jsonString);
            return areAllFieldsEmpty(jsonNode);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    private boolean areAllFieldsEmpty(JsonNode jsonNode) {
        if (jsonNode.isObject()) {
            Iterator<Map.Entry<String, JsonNode>> fields = jsonNode.fields();
            while (fields.hasNext()) {
                Map.Entry<String, JsonNode> field = fields.next();
                JsonNode fieldValue = field.getValue();
                if (!fieldValue.isNull() && !fieldValue.isTextual()) {
                    return false;
                }
                if (fieldValue.isTextual() && !fieldValue.asText().isEmpty()) {
                    return false;
                }
                if (fieldValue.isContainerNode() && !areAllFieldsEmpty(fieldValue)) {
                    return false;
                }
            }
        } else if (jsonNode.isArray()) {
            for (JsonNode arrayElement : jsonNode) {
                if (arrayElement.isObject() && !areAllFieldsEmpty(arrayElement)) {
                    return false;
                }
            }
        }
        return true;
    }
}

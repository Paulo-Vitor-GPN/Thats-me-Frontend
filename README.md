import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

import io.swagger.annotations.ApiModel;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
@AllArgsConstructor
@ApiModel(description = "remove warranty b3")
public class RemoverGarantiaB3_Response_DTO {

    private String codigo;
    private String mensagem;
    private List<RemoverGarantiaB3_Response_Item_DTO> item;

    public List<RemoverGarantiaB3_Response_Item_DTO> getItem() {
        return Collections.unmodifiableList(item);
    }

    public void setItem(List<RemoverGarantiaB3_Response_Item_DTO> item) {
        this.item = Collections.unmodifiableList(new ArrayList<>(item));
    }
}

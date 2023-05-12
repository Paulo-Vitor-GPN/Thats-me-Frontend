package br.com.santander.bhs.vjcaml.clientintegration.dto.removerGarantiaB3;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

import io.swagger.annotations.ApiModel;

import lombok.Generated;
import lombok.Getter;
import lombok.Setter;

/**
 *
 * @author T769118
 *
 */
@Getter
@Setter
@Generated
@ApiModel(description = "remove warranty b3")
public class RemoverGarantiaB3_Response_DTO {

	private String codigo;
	private String mensagem;


	public List<RemoverGarantiaB3_Response_Item_DTO> getItem() {
		List<RemoverGarantiaB3_Response_Item_DTO> it = item;
		return it;
	}

	public void setItem(List<RemoverGarantiaB3_Response_Item_DTO> item) {
		item = new ArrayList<RemoverGarantiaB3_Response_Item_DTO>(item);
		this.item = Collections.unmodifiableList(item);
	}

}

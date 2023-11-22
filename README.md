import java.util.List;

public class Dados {
    private List<Item> data;

    public List<Item> getData() {
        return data;
    }

    public void setData(List<Item> data) {
        this.data = data;
    }
}

public class Item {
    private String codigoGrupo;
    private int codigoCota;
    private int versao;
    private String nomeConsorciado;
    private String primeiroNome;
    private String email;
    private String dddCelular;
    private String numeroCelular;
    private String cpfCnpj;
    private String tipoPessoa;
    private String penumper;
    private String codigoProduto;
    private double valorBem;
    private double valorPrimeiraParcela;
    private String formaAcesso;

    // Inclua getters e setters para todos os campos

    // Exemplo de getters e setters para alguns campos:
    public String getCodigoGrupo() {
        return codigoGrupo;
    }

    public void setCodigoGrupo(String codigoGrupo) {
        this.codigoGrupo = codigoGrupo;
    }

    public int getCodigoCota() {
        return codigoCota;
    }

    public void setCodigoCota(int codigoCota) {
        this.codigoCota = codigoCota;
    }

    // Continue criando getters e setters para os outros campos
}

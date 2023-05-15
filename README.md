listFillment(params.getPremios().getPremio(), requestTransacao);

// ...

static public void listFillment(List<Premio> premios, RequestTransacao requestTransacao) {
    for (int i = 1; i <= 10; i++) {
        String cdcot = "CDCOT" + String.format("%02d", i);
        String vlpre = "VLPRE" + String.format("%02d", i);

        Premio premio = premios.get(i);
        requestTransacao.setField(cdcot, premio.getCodigoCotacao());
        requestTransacao.setField(vlpre, new BigDecimal(premio.getValor()));
    }
}

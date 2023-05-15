List<Premio> premios = params.getPremios().getPremio();

for (int i = 1; i <= 10 && i < premios.size(); i++) {
    Premio premio = premios.get(i);
    String codigoCotacao = premio.getCodigoCotacao();
    BigDecimal valor = new BigDecimal(premio.getValor());

    String cdcot = "CDCOT" + String.format("%02d", i);
    String vlpre = "VLPRE" + String.format("%02d", i);

    try {
        Field cdcotField = requestTransacao.getClass().getDeclaredField(cdcot);
        Field vlpreField = requestTransacao.getClass().getDeclaredField(vlpre);

        cdcotField.setAccessible(true);
        vlpreField.setAccessible(true);

        cdcotField.set(requestTransacao, codigoCotacao);
        vlpreField.set(requestTransacao, valor);
    } catch (NoSuchFieldException | IllegalAccessException e) {
        // Lidar com a exceção adequadamente
    }
}

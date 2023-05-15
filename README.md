private void populaRequest(ParamsSimularParcelas params, VJME074 requestTransacao) {
    requestTransacao.setQTDPAFN(Integer.parseInt(params.getParcelas().getQuantidade()));
    requestTransacao.setQTDCALC(Integer.parseInt(params.getCalculo().getQuantidade()));
    requestTransacao.setVLFINAN(Integer.parseInt(params.getValorFinanciado()));
    requestTransacao.setFATORAT(Integer.parseInt(params.getCalculo().getFator()));
    requestTransacao.setCDPACOF(Integer.parseInt(params.getTabelaFinanciamento().getPacote()));

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

    requestTransacao.setVMUSUAR(user);
}

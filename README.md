List<PremioPremiosSimularParcelasType> premios = params.getPremios().getPremio();

if (premios.size() >= 1) {
    requestTransacao.setCDCOT(StringUtils.leftPad("01", 2, "0"), premios.get(1).getCodigoCotacao());
    requestTransacao.setVLPRE(StringUtils.leftPad("01", 2, "0"), new BigDecimal(premios.get(1).getValor()));
}

if (premios.size() >= 2) {
    requestTransacao.setCDCOT(StringUtils.leftPad("02", 2, "0"), premios.get(2).getCodigoCotacao());
    requestTransacao.setVLPRE(StringUtils.leftPad("02", 2, "0"), new BigDecimal(premios.get(2).getValor()));
}

// Continue definindo as propriedades para os demais elementos da lista conforme necessário

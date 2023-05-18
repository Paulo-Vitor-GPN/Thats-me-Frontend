if (!CollectionUtils.isEmpty(dtoResponse.getObjeto().getListaErros())) {
				PsError psErro = dtoResponse.getObjeto().getListaErros().get(0);
				ErroAltair erroAltair = AltairFormatToMessageConverter.convert(dtoResponse);
				exchange.getIn().setHeader(Exchange.HTTP_RESPONSE_CODE, HttpStatus.BAD_REQUEST.value());

				logger.info("RESPONSE: " + CommonUtils.ERROR_VJ740 + " - " + psErro.getMensagem());
				MensagemAltair mensagemAltair = new MensagemAltair();
				mensagemAltair.setErroAltair(erroAltair);
				responseDTO.setMensagemAltair(mensagemAltair);
				exchange.getMessage().setBody(responseDTO);
				return exchange;

			}

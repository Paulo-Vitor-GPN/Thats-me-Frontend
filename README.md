public static String createReqGetSignture(String pathToPem, String systemAcronym) throws Exception {
		byte[] pemContent = PEMSignature.readPemFile(pathToPem);
		PEMSignatureResponse response;
		response = PEMSignature.generate(systemAcronym, pemContent);
		JSONObject json = new JSONObject();
		json.put("system", response.getSystem());
		json.put("timestamp", response.getTimestamp());
		json.put("nonce", response.getNonce());
		json.put("signature", response.getSignature());

		return json.toString();

	}

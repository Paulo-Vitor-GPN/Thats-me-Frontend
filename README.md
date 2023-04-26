@Test
	public void createReqGetSigntureTest() {
		String pathToPem = "";
		String systemAcronym = "EM";
		when(PEMSignature.readPemFile(pathToPem)).thenReturn(PEMSignature.readPemFile(""));
		try {
			String str = CommonUtils.createReqGetSignture(pathToPem, systemAcronym);
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

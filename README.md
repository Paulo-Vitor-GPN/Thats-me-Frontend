@Test
public void createReqGetSignatureTest() throws IOException {
    // Arrange
    String pathToPem = "";
    String systemAcronym = "EM";
    when(PEMSignature.readPemFile(anyString())).thenReturn(PEMSignature.readPemFile(""));
    
    // Act
    String str = CommonUtils.createReqGetSignature(pathToPem, systemAcronym);
    
    // Assert
    assertNotNull(str);
    // add more assertions if necessary
}

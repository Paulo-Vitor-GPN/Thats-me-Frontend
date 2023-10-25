ObjectMapper objectMapper = new ObjectMapper();

    Builder csvSchemaBuilder = CsvSchema.builder();

    JsonNode jsonNode = null;
    try {
      StringBuilder stringBuilder = new StringBuilder();
      try (BufferedReader reader = new BufferedReader(new FileReader(jsonFile))){
        String line;
        while ((line = reader.readLine()) != null){
        	line = line.replace("\\n", "").replace("\\r", "");
          stringBuilder.append(line);
        }
      }
      String jsonContent = stringBuilder.toString();
      jsonTree = objectMapper.readTree(jsonContent);
      jsonNode = jsonTree.elements().next();
      jsonNode
              .fieldNames()
              .forEachRemaining(
                      fieldName -> {
                        csvSchemaBuilder.addColumn(fieldName);
                      });
    } catch (Exception e) {
      e.printStackTrace();
    } finally {
      this.createCsvFile(jsonFile, jsonTree, csvSchemaBuilder);
    }

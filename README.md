Baixar Marcação
API PUT /api/v1.0/contratos/{numeroContrato}/baixa Header: Content-type: application/json Authorization: Bearer {TOKEN} Body: { “chassiLista”: array<string> } Responses: 200: <sem corpo> 40x (casos de erro): { “codigo”: string, “mensagem”: string } 422 (casos de erro): [ { “chassi”: string, “erro”: { “codigo”: string, “mensagem”: string } }, … ]

Baixar Marcação – Exemplo
Produção PUT https://api-veiculos-std.b3.com.br/FLPLB/api/v1.0/contratos/ CACTUS6074/baixa Produção PUT https://api-veiculos.b3.com.br/FLPLB/api/v1.0/contratos/ CACTUS6074/baixa Homologação PUT https://api-veiculos-cert-std.b3.com.br/FLPLB/api/v1.0/contratos/ CACTUS6074/baixa Homologação PUT https://api-veiculos-cert.b3.com.br/FLPLB/api/v1.0/contratos/ CACTUS6074/baixa Authorization: Bearer {access_token} Content-type: application/json Request Body: { "chassiLista": [ "8AFFZZFHA9J270847", "KMHDC51EACU386440", "9BWAB05U8AP045941" ] } Response Body (Exemplo Sucesso): N/A Response Body (Exemplo Erro 40X, ou 500): { "codigo": “404.1”, “mensagem”: “Contrato não encontrado”
} Response Body (Exemplo Erro 422): [ { “chassi”: “8AFFZZFHA9J270847”, “erro”: { “codigo“: “422.27”, “mensagem”: “Chassi não pertencente ao contrato” } }, { “chassi”: “9BWAB05U8AP045941”, “erro”: { “codigo“: “422.6”, “mensagem”: “Chassi inválido” } }
]

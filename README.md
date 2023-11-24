{
    "systemId": "MSG", //Conta cadastrada no Portal MSG
    "stimulumId": "MSGEMXXXXX", //Estímulo cadastrado no Portal do MSG e vinculado a conta cadastrada no systemId
    "variables": [
        {
            "key": "1", //Chave Obrigatória - Destinatário
            "value": "fulano@santander.com.br"
        },
        {
            "key": "2", //Chave não obrigatória - Remetente - Caso a mesma não seja informada o rementente default cadastrad no estímulo será utilizado.
            "value": "santander@santander.com.br"
        },
        {
            "key": "3", //Chave não obrigatória - Caso a mesma não seja informada o assunto será relacionado conforme cadastrado no estímulo.
            "value": "Assunto do E-mail encaminhado"
        }
    ]
}

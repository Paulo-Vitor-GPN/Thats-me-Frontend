<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:v1="http://model.santander.com.br/bsb/criptografia/v1"
    xmlns:v11="http://model.santander.com.br/bsb/mensagem/v1"
    xmlns:v12="http://services.santander.com.br/bsb/santanderfinanciamentos/propostacreditosantanderfinanciamentos/v1">
    <soapenv:Body>
        <v12:simularParcelas>
            <parcelas>
                <quantidade xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
            </parcelas>
            <calculo>
                <quantidade xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
                <fator xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
            </calculo>
            <valorFinanciado xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
            <tabelaFinanciamento>
                <pacote xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
            </tabelaFinanciamento>
            <premios>
                <premio>
                    <codigoCotacao xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
                    <valor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
                </premio>
            </premios>
        </v12:simularParcelas>
    </soapenv:Body>
</soapenv:Envelope>

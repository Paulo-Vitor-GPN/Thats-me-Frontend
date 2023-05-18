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
                <quantidade>10</quantidade>
                <fator>1.5</fator>
            </calculo>
            <valorFinanciado>5000</valorFinanciado>
            <tabelaFinanciamento>
                <pacote>XYZ</pacote>
            </tabelaFinanciamento>
            <premios>
                <premio>
                    <codigoCotacao>ABC123</codigoCotacao>
                    <valor>null</valor>
                </premio>
            </premios>
        </v12:simularParcelas>
    </soapenv:Body>
</soapenv:Envelope>

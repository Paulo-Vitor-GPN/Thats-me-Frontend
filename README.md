package br.com.santander.yzcaml.clientintegration.builders.entities;

import br.com.santander.yzcaml.clientintegration.builders.EmailBuilder;
import br.com.santander.yzcaml.clientintegration.builders.FallbackBuilder;
import br.com.santander.yzcaml.clientintegration.builders.PushBuilder;
import br.com.santander.yzcaml.clientintegration.builders.SmsBuilder;
import com.fasterxml.jackson.databind.ObjectMapper;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.SneakyThrows;

import java.util.HashSet;
import java.util.Set;

@NoArgsConstructor
@Data
public class Stimulus {

    private String stimulusId;
    private String systemId;
    private Set<Variable> variables = new HashSet<>();

    public static SmsBuilder smsBuilder() {
        return new SmsBuilder();
    }

    public static EmailBuilder emailBuilder() {
        return new EmailBuilder();
    }

    public static PushBuilder pushBuilder() {
        return new PushBuilder();
    }

    public static FallbackBuilder fallbackBuilder() {
        return new FallbackBuilder();
    }

    @SneakyThrows
    public String toJsonString() {
        return new ObjectMapper().writeValueAsString(this);
    }

}




package br.com.santander.yzcaml.clientintegration.builders.entities;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.EqualsAndHashCode;

@AllArgsConstructor
@Data
@EqualsAndHashCode(of = "key")
public class Variable {

    private String key;
    private String value;

}



package br.com.santander.yzcaml.clientintegration.builders;

import br.com.santander.yzcaml.clientintegration.builders.entities.Stimulus;
import br.com.santander.yzcaml.clientintegration.builders.entities.Variable;
import br.com.santander.yzcaml.clientintegration.builders.exception.StimulusBuildException;

import java.util.Set;

public abstract class StimulusBuilder {

    private Stimulus stimulus = new Stimulus();

    public StimulusBuilder stimulusId(String stimulusId) {
        this.stimulus.setStimulusId(stimulusId);
        return this;
    }

    public StimulusBuilder systemId(String systemId) {
        this.stimulus.setSystemId(systemId);
        return this;
    }

    public StimulusBuilder variables(Set<Variable> variables) {
        this.stimulus.setVariables(variables);
        return this;
    }

    public StimulusBuilder addVariable(String key, String value) {
        this.stimulus.getVariables().add(new Variable(key, value));
        return this;
    }

    public Stimulus build() {
        if(this.stimulus.getStimulusId() == null || this.stimulus.getSystemId() == null) {
            throw new StimulusBuildException("StimulusId and SystemId properties must not be null.");
        }
        return this.stimulus;
    }

    protected Boolean hasVariable(String key) {
        return this.stimulus
                .getVariables()
                .contains(new Variable(key, ""));
    }

}


package br.com.santander.yzcaml.clientintegration.builders;

import br.com.santander.yzcaml.clientintegration.builders.entities.Stimulus;
import br.com.santander.yzcaml.clientintegration.builders.entities.Variable;
import br.com.santander.yzcaml.clientintegration.builders.exception.StimulusBuildException;

import java.util.Set;

public class EmailBuilder extends StimulusBuilder {

    public EmailBuilder to(String to) {
        this.addVariable("1", to);
        return this;
    }

    public EmailBuilder from(String from) {
        this.addVariable("2", from);
        return this;
    }

    public EmailBuilder subject(String subject) {
        this.addVariable("3", subject);
        return this;
    }

    public EmailBuilder cc(String cc) {
        this.addVariable("4", cc);
        return this;
    }

    public EmailBuilder attachment(String attachment) {
        this.addVariable("gnid", attachment);
        return this;
    }

    @Override
    public EmailBuilder stimulusId(String stimulusId) {
        return (EmailBuilder) super.stimulusId(stimulusId);
    }

    @Override
    public EmailBuilder systemId(String systemId) {
        return (EmailBuilder) super.systemId(systemId);
    }

    @Override
    public EmailBuilder variables(Set<Variable> variables) {
        return (EmailBuilder) super.variables(variables);
    }

    @Override
    public EmailBuilder addVariable(String key, String value) {
        return (EmailBuilder) super.addVariable(key, value);
    }

    @Override
    public Stimulus build() {
        if(!this.hasVariable("1")){
            throw new StimulusBuildException("'to' variable must not be null.");
        }

        return super.build();
    }
}


package br.com.santander.yzcaml.clientintegration.builders;

import br.com.santander.yzcaml.clientintegration.builders.exception.StimulusBuildException;
import lombok.AllArgsConstructor;

@AllArgsConstructor
public class FallbackEmailBuilder {

    private FallbackBuilder fallbackBuilderRefactored;

    public FallbackEmailBuilder from(String from) {
        fallbackBuilderRefactored.addVariable("remetenteEmail", from);
        return this;
    }

    public FallbackEmailBuilder to(String to) {
        fallbackBuilderRefactored.addVariable("destinatarioEmail", to);
        return this;
    }

    public FallbackEmailBuilder subject(String subject) {
        fallbackBuilderRefactored.addVariable("assuntoEmail", subject);
        return this;
    }

    public FallbackEmailBuilder attachment(String attachment) {
        fallbackBuilderRefactored.addVariable("gnid", attachment);
        return this;
    }

    public FallbackBuilder end() {
        if(!fallbackBuilderRefactored.hasVariable("destinatarioEmail")){
            throw new StimulusBuildException("'to' variable must not be null.");
        }

        return this.fallbackBuilderRefactored;
    }

}

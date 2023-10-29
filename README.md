# NLICE: a medically standardized symptom modeling approach
 NLICE symptom modeling is a novel way to enhance the symptom representation, which is therefore useful for identifying diseases that have similar symptoms. Furthermore, in collaboration with medical experts, medically correct symptom-condition statistics are provided to increase the accuracy of simulated patient records. The NLICE dataset uses statistics provided by medical experts, and includes 55 conditions each with 137 potential symptoms. In this way, we can bridge the gap between limited available patient records and data-driven healthcare methodologies.
## Dataset format
We generated totally around 1000k patients record in ./sample_data folder. For each record, the presented symptom is associated with
eight NLICE features: [symptom : nature : location :
intensity : frequency : duration : onset : excitation].
However, each condition does not need to contain all
NLICE features. 
### example record
| GENDER | RACE  | AGE | PATHOLOGY                                                               | SYMPTOMS                                                                                                                                                                                                          |
|--------|-------|-----|-------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| F      | Asian | 30  | Acute pancreatitis                                                      |Discoloration:Yellow(Jaundice):::::::; Altered appetite:Decreased(anorexia):::::::;Pain:Radiating:::::::; Nausea::::::::;Flatulence::::::::; Bloating:::::::: |
| F      | White | 48  | Rheumatoid arthritis feminine| Disability::::::::; Altered appetite:Decreased(Anorexia):::::::; Fatigue::::::::; Stiffness::::::Morning::;Weight loss::::::::     


## Nature

 For example, we use coughing as an illustration, Modeling this symptom using a one-or-zero approach ignores the various ways coughing may occur. A dry cough could be experienced by some patients as opposed to others who may cough up mucous. These variations specify the type of cough and offer details that might make it simpler to differentiate between conditions that share similar symptoms.

## Location

Location refers to the position on the body where a patient experiences a symptom. The location of a symptom can be a discriminating factor when making distinctions between conditions. For example, we consider a patient who reports experiencing abdominal pain. In terms of medical anatomy, the abdomen can be divided into upper and lower right quadrants as well as upper and lower left. 


## Intensity

The intensity of a symptom refers to the severity at which a symptom is experienced. The severity of the symptom presentation may clearly indicate the underlying illness. For instance, a common symptom of many ailments is pain. An appendicitis sufferer will commonly experience severe to moderate abdominal discomfort.
 

## Chronology

Chronology encompasses three different concepts: 1.~frequency, which refers to how frequently a symptom occurs, 2.~duration, which refers to how long the symptom lasts, and 3.~onset, which describes the time the patient first noticed the symptom. When determining a differential diagnosis, these characteristics may provide diagnostic value.

## Excitation

Activities that patients engage in or situations patients are exposed to that activate or worsen the symptoms are referred to as excitation. For instance, a patient may only experience heart pain while swimming. We could also distinguish conditions more precisely if we recorded excitation information.

## Data Generation Pipeline

### Generating Synthea Modules

Synthea allows the modeling of conditions using generic modules. Also recall that the main aim of including Symcat was to replace the missing condition-symptom probabilistic relationships present by default in Synthea.

To this end, a generator was created which is able to take as input the `conditions.json` input file in this repo - which contain the condition and symptom definitions, the probabilistic relationship between them and their dependence on patient demography (age, sex, race) - and generate valid Synthea Modules.

To generate these modules the following command is run:

### Generating NLICE Synthea Modules

 the command for generating Synthea compatible modules is then:

```bash
cd <path-to-repos>/symcat-to-synthea
./main.py --gen_modules --conditions_json --output output/modules --generator_mode 5 --prefix nlice_
```
the symcat-to-synthea pipeline could be found in [symcat-to-synthea](https://github.com/teliov/symcat-to-synthea).
#### **Module Generator Configuration Options**

**--min_symptoms**:
Since the symptoms expressed by a sick patient are selected probabilistically, it is possible that a sick patient presents with no symptom. This configuration ensures that a minimum number of symptoms is presented by a sick patient. The default is set to 1. Once the generation process is complete and the number of symptoms included are not up to the minimum number of specified symptoms, then the generator automatically inserts symptoms with the most likely symptom being inserted first until the minimum number is reached.

**--prefix**:
This option allows the specification of a prefix for the generated modules. For this project the prefix *symcat_* was used. In this case, given a disease e.g. *appendicitis* then the generated module would be *symcat_appendicitis*. This allows an easy distinction between modules generated using Symcat data and Synthea's default modules. The importance of this distinction would be apparent in the section on generating data.

#### **Synthea Version**

To reproduce the data generated during this project, ./NLICE_Synthea should be used. Slight modifications were made to the format of the exported data. This version is also behind the current master version of Synthea (as at the time of writing), hence using this forked version would reduce the chances of errors or mismatch due to changes introduced in the repo.

#### **Running Synthea**

The command for generating data is given below:
```bash
cd <synthea-repo>
./run_synthea -p <num_patients> --exporter.fhir.export=false\
    --exporter.practitioner.fhir.export=false\
    --exporter.symptoms.csv.export=true\
    --exporter.symptoms.mode=1\
    --exporter.years_of_history=0\
    -m symcat_*\
    -d <path-to-generated-modules>\
    --exporter.baseDirectory=<path-to-data-output>
```


**Run-Through of Active Options**

**-p <num_patients>**: This specifies the number of patients to be generated. Note that since one patient can have multiple conditions, it does not determine the number of diagnosed conditions which would be generated. However, the number of conditions generated is positively correlated with the number of patients specified.

**--exporter.practitioner.fhir.export=false**: This disables - in this case - an *[FHIR](http://hl7.org/fhir/)* export of generated patients. For this project, the desired output was CSV and Synthea defaults to *FHIR*.

**--exporter.symptoms.mode=1**: This instructs Synthea to create a CSV file of the symptoms. By default this is turned off i.e. set to 0.

**--exporter.years_of_history=0**: This instructs Synthea to export data for a patients entire lifetime. By setting a non-negative value *n*, it is possible to instruct Synthea to restrict the export to the last *n* available years of the patient's data.

**-m symcat_**: This specifies a module regex which Synthea uses in selecting which disease modules would be active during the generation process. By specifying *symcat_* we can target only those modules generated using the `symcat-to-synthea` generator as described earlier. Synthea's default modules are thus excluded.

**-d \<path-to-generated-modules\>**: This specifies the path to the generated modules. By default Synthea  bundles its own modules (e.g in the generated jar file) and uses those when generating. This flag instructs the generator to search for modules in the specified directory.

**--exporter.baseDirectory=\<path-to-data-output\>**: This specifies the output location of generated data.

A sample output directory structure is shown below:
```
./output
└── symptoms
    └── csv
        └── symptoms.csv

2 directories, 1 file 




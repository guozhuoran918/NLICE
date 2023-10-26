# NLICE: a medically standardized symptom modeling approach
 NLICE symptom modeling is a novel way to enhance the symptom representation, which is therefore useful for identifying diseases that have similar symptoms. Furthermore, in collaboration with medical experts, medically correct symptom-condition statistics are provided to increase the accuracy of simulated patient records. The NLICE dataset uses statistics provided by medical experts, and includes 55 conditions each with 137 potential symptoms. In this way, we can bridge the gap between limited available patient records and data-driven healthcare methodologies.
## Dataset format
We generated totally around 1000k patients record.For each record, the presented symptom is associated with
eight NLICE features: [symptom : nature : location :
intensity : frequency : duration : onset : excitation].
However, each condition does not need to contain all
NLICE features. 
### example record
| GENDER | RACE  | AGE | PATHOLOGY                                                               | SYMPTOMS                                                                                                                                                                                                          |
|--------|-------|-----|-------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| F      | Asian | 30  | Acute pancreatitis                                                      |Discoloration:Yellow(Jaundice):::::::; Altered appetite:Decreased(anorexia):::::::;Pain:Radiating:::::::; Nausea::::::::;Flatulence::::::::; Bloating:::::::: |
| F      | White | 48  | Rheumatoid arthritis feminine| Disability::::::::; Altered appetite:Decreased(Anorexia):::::::; Fatigue::::::::; Stiffness::::::Morning::;Weight loss::::::::     
|

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



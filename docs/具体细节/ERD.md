# ERD 图

```mermaid
erDiagram
patients{
    primaryKey _id
    string id 
    string name
    int gender
    int isNormal
    bool coordination
    string coodinationDesc
    string IDcard
    date birth
    int handedness
    int educated
    int educatedYears
    int livingEnv
    int yearEducatedFift
    todo asDescribedInDoc
}


doctors{
    autoincrement id
    string name
    string account
    int password
    string indexPath
}

diagnosis{
    int id
    foreign_int doctorID
    foreign_int patientID
    todo moreInfo
}

machineDiagnoses{
    int id
    foreign_int patientID
    todo moreInfo
}

template{
    int id
    int doctorID
    int inuse
    string name
    json quiz
}

answ{
    int id
    foreign_int patient
    json answ
    todo additionalInfo
}

ownership{
    int id
    foreign_int doctor
    foreign_int patient
    int field
}


patients || -- o{ answ: answer
answ }o -- || template:template
doctors || -- o{ template: use
patients || -- o{ diagnosis:have
patients || -- o{ machineDiagnoses:have
ownership }o -- || doctors: owns
ownership }o -- || patients: target

```
<!-- ## 问卷部分

```mermaid
erDiagram

``` -->

<!-- ```mermaid
erDiagram

quizTemplate{
    autoincrement id
    foreign_int doctorID
    string name
}

quizResult{
    autoincrement id
    foreign_int patientID
    foreign_int doctorID
    todo aboutResult
}

fillQuiz{
    autoincrement id
    foreign_int templateID
    string desc
}

fillAnsw{
    autoincrement id
    string desc
    string answ
}

choiceQuiz{
    autoincrement id
    foreign_int templateID
    string desc
    int type
    string choices
}

choiceAnsw{
    autoincrement id
    string desc
    string choices
    int answ
}

fillChoiceAnsw{
    autoincrement id
    string desc
}





``` -->
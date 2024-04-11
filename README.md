```
@startuml
package "Domain" {
  package "Aggregates" {
    [AgregateFactory]
    [Agregate]
    cloud AgregateVoter
    note bottom of AgregateVoter
      Check validity before the operation
      Could modify the entity values
    end note

    cloud AgregateNotifier
    note bottom of AgregateNotifier
      Notify the result of the operation
    end note
  }
  package "Requires" {
    () Gateway
  }
  package "Model" {
    [Entity]
  }
  Agregate --> Gateway
  Agregate --> Entity
  AgregateFactory --> Agregate : create
  Agregate --> AgregateVoter : <<N>>
  Agregate --> AgregateNotifier : <<N>>
  note left of AgregateFactory
    Hold the Aggregate dependencies to build
  end note
}

package "Aplication" {
  package "Exposes" {
    () Usecase
  }
  package "Interactors" {
    [Interactor]
    cloud UsecaseVoter
    cloud UsecaseNotifier
  }
  Interactor -- Usecase
  Interactor ..> AgregateFactory : build
  Interactor --> Agregate : use
  Interactor --> UsecaseVoter : <<N>>
  note bottom of UsecaseVoter
     Check validity before the operation
      Could modify the entity values
  end note
  Interactor --> UsecaseNotifier : <<N>>
  note bottom of UsecaseNotifier
      Notify the result of the operation
  end note
}


package "Infra" {
  package "Rest" {
    [Controller]
    Controller ----> Usecase
  }
  package "Adapters" {
    [Adapter]
    Adapter ---- Gateway
  }
}
@enduml

```

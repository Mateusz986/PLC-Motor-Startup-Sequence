# **PLC-Motor-Startup-Sequence**
Automat stanów (state machine) sterujący sekwencją rozruchową silnika przemysłowego, napisany w języku Structured Text (ST) w środowisku CODESYS. Projekt symuluje realną logikę bezpiecznego uruchamiania maszyny: pompa oleju → zawór paliwa → praca, z obsługą awarii i wizualizacją HMI.

<img width="911" height="445" alt="image" src="https://github.com/user-attachments/assets/80f450a4-1ef4-4a57-8106-5c0408428d99" />

## **Dlaczego kolejność ma znaczenie**
To nie jest przypadkowa sekwencja - odzwierciedla realną logikę bezpieczeństwa silników przemysłowych: najpierw ustawić smarowanie silnika, by zapobiec awarii, a następnie wpuścić paliwo. Uruchomienie silnika bez ustalonego ciśnienia oleju grozi zatarciem. Timer 5s w stanie pompy oleju zapobiega temu symulując czas ustalenia smarowania.

## **Diagram stanów**

```mermaid
stateDiagram-v2
    [*] --> WaitingState
    WaitingState --> OilPumpState : xStartBtn
    OilPumpState --> ValveState : tmrOilDelay.Q
    OilPumpState --> WaitingState : xStopBtn
    ValveState --> WorkingState : xValveConfirm
    ValveState --> IssuesState : tmrValveTimer.Q
    ValveState --> WaitingState : xStopBtn
    WorkingState --> IssuesState : xFaultSensor
    WorkingState --> WaitingState : xStopBtn
    IssuesState --> WaitingState : xResetBtn AND NOT xFaultSensor
```


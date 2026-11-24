flowchart TD
    %% СТИЛИ
    classDef pathway fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef protein fill:#fff,stroke:#333,stroke-width:1px;
    classDef inhibitor fill:#ffe6cc,stroke:#d79b00,stroke-width:1px;
    classDef complex fill:#d5e8d4,stroke:#82b366,stroke-width:1px;
    classDef legend fill:#fff,stroke:#666,stroke-width:1px,stroke-dasharray: 5 5;

    %% КЛАССИЧЕСКИЙ ПУТЬ
    subgraph Классический ["Классический путь"]
        direction TB
        AgAb["Комплексы антиген-антитело"] --> C1q["C1q"]
        C1q --> C1s["C1s"]
        C1q --> C1r["C1r"]
        
        C1s --> C4["C4"]
        C4 --> C4b["C4b"]
        C4 --> C4a["C4a"]
        
        C1s --> C2["C2"]
        C2 --> C2a["C2a"]
        C2 --> C2b["C2b"]
        
        C4b --> C3ConvClassic["C3 конвертаза (C4b2a)"]
        C2a --> C3ConvClassic
    end

    %% АЛЬТЕРНАТИВНЫЙ ПУТЬ
    subgraph Альтернативный ["Альтернативный путь"]
        direction TB
        C3_alt["C3"] --> TickOver["спонтанная активация"]
        TickOver --> C3H2O["C3(H2O)"]
        
        C3H2O --> FB["FB"]
        FB --> FluidConv["растворимая С3 конвертаза (C3(H2O)Bb)"]
        FD["FD"] --> FluidConv
        MASP3["MASP-3"] --> FD
    end

    %% ОБЩИЙ УЗЕЛ АКТИВАЦИИ C3
    C3ConvClassic --> C3_main["C3"]
    FluidConv --> C3_main
    C3_main --> C3a["C3a"]
    C3_main --> C3b["C3b"]

    %% ПЕТЛЯ УСИЛЕНИЯ И ПОВЕРХНОСТНАЯ КОНВЕРТАЗА
    C3b --> SurfaceConv["поверхностно-связанная С3 конвертаза (C3bBb)"]
    FB --> SurfaceConv
    SurfaceConv -.->|петля амплификации| C3_main
    SurfaceConv --> C3b
    C3b --> C5ConvAlt["С5 конвертаза (C3bBb3b)"]
    SurfaceConv --> C5ConvAlt

    %% С5 КОНВЕРТАЗА (КЛАССИЧЕСКАЯ)
    C3b --> C5ConvClassic["С5 конвертаза (C4b2a3b)"]
    C3ConvClassic --> C5ConvClassic

    %% ТЕРМИНАЛЬНЫЙ ПУТЬ (MAC)
    C5ConvClassic --> C5_main["C5"]
    C5ConvAlt --> C5_main
    C5_main --> C5a["C5a"]
    C5_main --> C5b["C5b"]
    
    C5b --> MAC["C5b9 / MAC"]
    C6["C6"] --> MAC
    C7["C7"] --> MAC
    C8["C8"] --> MAC
    C9["C9"] --> MAC

    %% ИНГИБИТОРЫ И РЕГУЛЯТОРЫ
    FH["FH"] -->|ингибирование| C3b
    CD46["CD46 (MCP)"] -.->|ингибирование| SurfaceConv
    CD46 -.->|ингибирование| C3b
    
    %% ДОБАВЛЕННЫЕ СТРЕЛКИ
    FH -.-> vWF["расщепление vWF"]
    C3a -.-> sFlt["стимуляция синтеза sFlt"]
    MAC -.-> sFlt

    %% ЛЕГЕНДА
    subgraph Обозначения ["Обозначения"]
        direction LR
        L1["образование комплекса"] -.-> L2["распад"]
        L3["протеолиз"] -.-> L4["ингибирование"]
    end

    %% ПРИМЕНЕНИЕ СТИЛЕЙ
    class Классический,Альтернативный,Обозначения pathway;
    class C1q,C1s,C1r,C4,C4a,C4b,C2,C2a,C2b,C3_alt,C3H2O,FB,FD,MASP3,C3_main,C3a,C3b,C5_main,C5a,C5b,C6,C7,C8,C9 protein;
    class FH,CD46 inhibitor;
    class C3ConvClassic,FluidConv,SurfaceConv,C5ConvAlt,C5ConvClassic,MAC complex;

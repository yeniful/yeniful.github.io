+++
title = '[Apple Platforms] SoC (System on a Chip)와 AP (Application Processor)'
date = 2026-02-03T00:00:00+09:00
draft = false
tags =  ["Apple Platforms", "CS"]
categories = ["Apple Platforms"]
summary = ""
+++

## Motivation
- 컴퓨터 구조를 공부하면서 스마트폰의 AP, SoC가 어떤 것에 대응되는지 궁금.

## Overview
- 컴퓨터의 CPU와 대응되는 것이 스마트폰의 AP, SoC ?
	- 하지만 컴퓨터 구조와 스마트폰에서의 컴퓨터 구조는 약간 다름
	- Apple Silicon이 SoC에 해당
- 목차
	- 컴퓨터 시스템 구조
	- SoC와 AP의 관계
	- Apple Silicon (Apple SoC)
	- iPhone 구조와 PC 구조의 대략적인 비교
	- Apple Silicon 구조
	- 정리

## 컴퓨터 시스템 구조
```mermaid
flowchart LR
    subgraph COMPUTER["Computer"]
        CPU["CPU"]
        MODE["Mode<br/>Bit"]
        DMA["DMA<br/>Controller"]
        TIMER["Timer"]
        MEM_CTRL["Memory<br/>Controller"]
        
        subgraph MEMORY["Memory"]
            PROG1["Program"]
            PROG2["Program"]
            OS["O/S"]
        end
        
        CPU --- MODE
        MODE --- DMA
        DMA --- TIMER
        MODE --- MEM_CTRL
        MEM_CTRL --- MEMORY
    end
    
    subgraph IO["I/O Device"]
        subgraph HD["Hard Disk"]
            DC1["Device<br/>Controller"] --- BUF1["Buffer"]
        end
        subgraph KB["Keyboard"]
            DC2["Device<br/>Controller"] --- BUF2["Buffer"]
        end
        subgraph PR["Print"]
            DC3["Device<br/>Controller"] --- BUF3["Buffer"]
        end
        subgraph MO["Monitor"]
            DC4["Device<br/>Controller"] --- BUF4["Buffer"]
        end
    end
    
    TIMER --- DC1
    TIMER --- DC2
    TIMER --- DC3
    TIMER --- DC4
    
    style COMPUTER fill:#fff,stroke:#1976d2,stroke-width:2px
    style IO fill:#fff,stroke:#1976d2,stroke-width:2px
    
    style CPU fill:#ffcdd2,stroke:#c62828,stroke-width:2px
    style MEMORY fill:#ffcdd2,stroke:#c62828,stroke-width:2px
    style HD fill:#ffcdd2,stroke:#c62828,stroke-width:2px
    
    style MODE fill:#fff,stroke:#333
    style DMA fill:#fff,stroke:#333
    style TIMER fill:#fff,stroke:#333
    style MEM_CTRL fill:#fff,stroke:#333
    style KB fill:#e8f5e9,stroke:#388e3c
    style PR fill:#e8f5e9,stroke:#388e3c
    style MO fill:#e8f5e9,stroke:#388e3c
```
- 컴퓨터 시스템 구조:
	- 컴퓨터
	- I/O 디바이스
- 우리가 흔히 말하는 [컴퓨터 구조](https://wikidocs.net/111325) 핵심 요소:
	- CPU (중앙처리장치)
	- 메모리 (주기억장치)
	- 디스크 (보조기억장치)

## SoC와 AP의 관계
- **SoC (System on Chip)**
	- '시스템을 한 칩에 통합'
	- CPU/GPU/NPU(신경망)/ISP(카메라)/DSP/메모리 컨트롤러/보안 영역 같은 것들이 들어있는 **통합 칩 전체**
- **AP (Application Processor)**
	- '앱을 돌리는 메인 프로세서(연산 중심)'
	- 모바일 업계에서 보통 메인 칩(=사실상 SoC)을 AP라고 부르는 관행이 생김.
	- 특히 한국/스마트폰 맥락에서 “AP 뭐야?” 하면 대개 **그 폰의 메인 SoC**를 뜻함.
- 퀄컴 Snapdragon, 삼성 Axynos를 보통 “AP”라고도 부르지만, 기술적으로는 전형적인 SoC
- Apple은 문서/기술 설명에서 SoC라는 표현을 많이 사용하며, 공식 용어집에서 SoC를 “여러 구성요소를 단일 칩으로 통합한 IC”로 정의 [Apple 용어집 참고](https://support.apple.com/ko-kr/guide/security/sec93292bfa6/1/web/1)

## Apple Silicon (Apple SoC)
- Apple의 SoC
	- A-시리즈(iPhone)
	- M-시리즈(Mac)
- Application Processor, Secure Enclave 등 구성 요소를 SoC 일부로 설명


## iPhone 구조와 PC 구조의 대략적인 비교
```mermaid
flowchart TB
    subgraph PC["🖥️ PC"]
        PC_CPU["CPU"]
        PC_GPU["GPU (별도)"]
        PC_RAM["RAM"]
        PC_SSD["SSD"]
    end
    
    subgraph iPhone["📱 iPhone"]
        subgraph AP["SoC ( ⊃ AP)"]
            IP_CPU["CPU"]
            IP_GPU["GPU"]
            IP_NPU["NPU"]
        end
        IP_RAM["RAM"]
        IP_FLASH["Flash Storage"]
    end
    
    style PC fill:#e3f2fd
    style iPhone fill:#fff8e1
    style AP fill:#fce4ec
```

## Apple Silicon 구조
```mermaid
graph TD
    subgraph SoC["SoC (System on Chip)"]
        AP["AP<br/>(CPU:<br/>P-core + E-core)"]:::cpu
        GPU["GPU"]:::gpu
        NPU["Neural Engine<br/>(NPU)"]:::npu
        ISP["ISP<br/>(Image Signal Processor)"]:::isp
        MEM["Memory Controller<br/>(UMA: LPDDR5X)"]:::mem
        SEC["Secure Enclave"]:::sec
        MOD["Modem<br/>(5G Baseband 등)"]:::mod
    end
    
    classDef cpu fill:#ffcdd2,stroke:#c62828,stroke-width:2px
    classDef gpu fill:#f3e5f5,stroke:#7b1fa2
    classDef npu fill:#e8f5e9,stroke:#388e3c
    classDef isp fill:#fff3e0,stroke:#f57c00
    classDef mem fill:#e0f2f1,stroke:#00695c
    classDef sec fill:#fce4ec,stroke:#c2185b
    classDef mod fill:#f5f5f5,stroke:#424242

```

## 정리
- 제품 스펙에서 **AP ≈ SoC** 로 흔하게 인식되지만
- 엄밀하게 구분하면 **AP는 SoC 중 앱/연산을 담당하는 중심 칩** (**SoC ⊃ AP**)
- Apple 플랫폼에서는 SoC 표현을 더 많이 사용

---

### References 👀
- [Apple SoC 보안](https://support.apple.com/ko-kr/guide/security/sec87716a080/web)
- [용어집](https://support.apple.com/ko-kr/guide/security/sec93292bfa6/1/web/1)
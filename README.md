# ULA de 5 bits em FPGA

Projeto da disciplina de **Sistemas Digitais**: uma Unidade Lógica e Aritmética (ULA) em lógica combinacional, desenhada em diagrama de blocos no **Quartus Prime** e implementada em uma FPGA **Cyclone IV E (EP4CE115F29C7)**, a da placa DE2-115.

## Visão geral

A ULA recebe dois operandos de 5 bits (`A` e `B`) e um seletor de operação de 3 bits (`S`). O resultado de 6 bits (`F`) e um sinal de `Status` aparecem nos LEDs e nos displays de 7 segmentos da placa.

| Sinal | Largura | Descrição |
|-------|---------|-----------|
| `A`, `B` | 5 bits | Operandos (bit 4 = sinal, bits 3..0 = magnitude) |
| `S` | 3 bits | Seleção da operação |
| `F` | 6 bits | Resultado |
| `Status` | 1 bit | Flag de status da operação |

## Operações

| `S2` | `S1` | `S0` | Função |
|:----:|:----:|:----:|--------|
| 0 | 0 | 0 | `F = A + B` |
| 0 | 0 | 1 | `F = A - B` |
| 0 | 1 | 0 | `F =` complemento a 2 de `B` |
| 0 | 1 | 1 | `F = (A = B)` |
| 1 | 0 | 0 | `F = (A > B)` |
| 1 | 0 | 1 | `F = (A < B)` |
| 1 | 1 | 0 | `F = A AND B` |
| 1 | 1 | 1 | `F = A XOR B` |

## Estrutura do projeto

```
PROJETO/
├── ProjetoSD.qpf / .qsf   # Projeto e configurações do Quartus (pinagem incluída)
├── integrar.bdf           # Top-level: ULA + decodificadores + displays
├── ULA.bdf                # Núcleo da ULA (somador, lógica, comparadores, MUXs)
├── dec_alu.bdf            # Decodificador de controle a partir de S
├── add_sub_unit.bdf       # Unidade de soma/subtração
├── full_adder.bdf         # Somador completo
├── adder_6bit.bdf         # Somador de 6 bits
├── c2_to_sm_6bit.bdf      # Conversão complemento de 2 -> sinal-magnitude
├── sm_to_2c_6bit.bdf      # Conversão sinal-magnitude -> complemento de 2
├── modulo_AND / modulo_XOR
├── comparadores_*.bdf     # Igual, maior que, menor que
├── MUX_*.bdf / mux4x1_*   # Multiplexadores de 1 e 6 bits
├── decodificador_7_segmentos_A/B.bdf, dec_segmento_soma.bdf, HEX*.bdf
├── *.sv                   # Módulos auxiliares em SystemVerilog (AND, comparador >)
├── Waveforms/             # Simulações funcionais (.vwf)
└── output_files/          # Relatórios de compilação e arquivo .sof
```

## Como usar

**Requisitos:** Quartus Prime 21.1.1 Lite (ou compatível) e uma placa DE2-115.

1. Abra `PROJETO/ProjetoSD.qpf` no Quartus.
2. Confirme que a entidade top-level é `integrar`.
3. Compile o projeto (`Processing > Start Compilation`).
4. Grave o `output_files/ProjetoSD.sof` na placa pelo Programmer. Se preferir, use o `.sof` já gerado.
5. Use as chaves para `A`, `B` e `S`, e observe o resultado nos LEDs e nos displays.

Para simular, abra os arquivos `.vwf` em `Waveforms/` no Quartus.

## Relatório de compilação

| Item | Valor |
|------|-------|
| Dispositivo | EP4CE115F29C7 (Cyclone IV E) |
| Elementos lógicos | 123 / 114.480 (< 1 %) |
| Registradores | 0 (circuito puramente combinacional) |
| Pinos | 84 / 529 (16 %) |

## Autores

- Andre Bessa da Costa
- Carolina Amarante Calderaro
- João Lucas Tavares Cavalcanti
- João Lucas Gomes da Silva

Disciplina de Sistemas Digitais, 2026.

# Конкатенация (объединение сигналов)

Конкатенация позволяет присвоить какому-то многоразрядному сигналу "склейку" из нескольких сигналов меньшей разрядности, либо наоборот: присвоить сигнал большей разрядности группе сигналов меньшей разрядности.

Оператор конкатенации выглядит следующим образом: `{sig1, sig2, ..., sign}`.

Предположим, у нас есть следующий набор сигналов:

![../../../technical/Other/Basic%20Verilog%20structures/Pic/concatenation/fig_01.drawio.png](../../../technical/Other/Basic%20Verilog%20structures/Pic/concatenation/fig_01.drawio.png)

```SystemVerilog

logic a;
logic b;
logic [7:0] c;
logic [1:0] d;

logic [5:0] e;
```

И мы хотим, чтобы на провод `e` подавались следующие сигналы:

- на старший бит сигнала `e` подавался сигнал `a`
- на его следующий бит подавался сигнал `b`
- на его следующие 2 бита подавались биты `[4:3]` сигнала `c`
- на младшие 2 бита подавался сигнал `d`

![../../../technical/Other/Basic%20Verilog%20structures/Pic/concatenation/fig_02.drawio.png](../../../technical/Other/Basic%20Verilog%20structures/Pic/concatenation/fig_02.drawio.png)

Это можно сделать путем 4 непрерывных присваиваний:

```SystemVerilog
logic a;
logic b;
logic [7:0] c;
logic [1:0] d;

logic [5:0] e;

assign e[5]   = a;
assign e[4]   = b;
assign e[3:2] = c[4:3];
assign e[1:0] = d;
```

либо через одно присваивание, использующее конкатенацию:

```SystemVerilog
logic a;
logic b;
logic [7:0] c;
logic [1:0] d;

logic [5:0] e;

assign e = {a, b, c[4:3], d};
```

Кроме того, возможна и обратная ситуация. Предположим, мы хотим подать отдельные биты сигнала `e` на различные провода:

![../../../technical/Other/Basic%20Verilog%20structures/Pic/concatenation/fig_02.drawio.png](../../../technical/Other/Basic%20Verilog%20structures/Pic/concatenation/fig_02.drawio.png)

```SystemVerilog
logic a;
logic b;
logic [7:0] c;
logic [1:0] d;

logic [5:0] e;

assign a      = e[5];
assign b      = e[4];
assign c[4:3] = e[3:2];
assign d      = e[1:0];
```

Подобную операцию можно так же выполнить в одно выражение через конкатенацию:

```SystemVerilog
logic a;
logic b;
logic [7:0] c;
logic [1:0] d;

logic [5:0] e;

assign {a, b, c[4:3], d} = e;
```

Кроме того, конкатенация может использоваться при **множественном дублировании** сигналов. Дублирование выполняется выражением:

```SystemVerilog
{a, {число_повторений{повторяемый_сигнал}} ,b}
```

Допустим, мы хотим присвоить какому-то сигналу три копии `[4:3]` битов сигнала `c`, после которых идут сигналы `a` и `b`.
Это можно сделать выражением:

```SystemVerilog
logic a;
logic b;
logic [7:0] c;

logic [7:0] e;

assign e = { {3{c[4:3]}}, a, b};
```


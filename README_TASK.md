 Постановка задачи

Обработать исключение Breakpoint выводом строки "break-break". Настроить ресет вектор и вектор обработки прерываний на 04800 и 0xA00 соответственно. Проверить работу программы на примере isa/rv32mi/sbreak.S.

| Номер варианта  | Вид исключения | Тест | Reset Vector | Trap Vector | Обработчик |
| --- | --- | --- | --- | --- |  --- |
| 10 | Breakpoint | isa/rv32mi/sbreak.S | 0x400 | 0xA00 | Вывод строки «break-break» |

# Ход работы

1. Создан рабочий репозиторий - fork от репозитория SCR1 https://github.com/syntacore/scr1

---

2. До внесения изменений проект запускается.

---

3.  Отредактировать список тестов, чтобы на выполнение остался только тест по варианту задания.


В файле со списком тестов *rv32_tests.inc* оставлен только тест *sbreak.S*.
Заданы переменные PATH и VERILATOR_ROOT.
```
make clean
make TARGETS="riscv_isa"
```

Вывод консоли:

---Test:                       sbreak.hex
break-breakTest passed

#--------------------------------------
# Summary: 1/1 tests passed
#--------------------------------------

- /home/dev/workspace/lab_scr1_sim/src/tb/scr1_top_tb_runtests.sv:181: Verilog $finish
Simulation performed on Verilator 5.010 2023-04-30 rev v5.010 
                          Test               | build | simulation 
                      sbreak.hex		OK	  PASS 

---

4. Модифицирована обработку исключений trap_vector в файле ./sim/tests/common/*riscv_macros.h*
5. В файле ./src/includes/*scr1_arch_description.svh* параметры ядра *Reset Vector* и *Trap Vector* установлены в соответствии с вариантом задания (0x400, 0x00).
6. Изменен linker-скрипт ./sim/tests/common/*link.ld*

```
...
----Test:                       sbreak.hex
break-breakTest passed

#--------------------------------------
# Summary: 1/1 tests passed
#--------------------------------------

- /home/dev/workspace/lab_scr1_sim/src/tb/scr1_top_tb_runtests.sv:181: Verilog $finish
Simulation performed on Verilator 5.010 2023-04-30 rev v5.010 
                          Test               | build | simulation 
                      sbreak.hex		OK	  PASS 

```

Можно видеть, что при обработке исключения было выведено сообщение "break-break".

Также в dump-файле *illegal.dump* адреса при `<_start>` и `<trap_vector>` соответствуют указанным в задании *Reset Vector* и *Trap Vector* соответственно.

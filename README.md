# PsychopyTrigger
Passo-a-passo para o setup do trigger nos experimentos dentro do Psychopy.

## Passo 1) Utilizando Psychopy Builder
Fig1 - Tela inicial do Psychopy Builder \
![Psychopy Builder](https://github.com/icaroafoliveira/PsychopyTrigger/blob/main/tela1.png)
- Crie um texto informando o paciente/participante que o experimento irá iniciar em instantes. Não esqueça de deixar o texto com tempo inicial, mas sem tempo final. \
    Exemplo:"O experimento irá se iniciar em alguns instantes" \
- Em seguida crie um componente para código (CODE component) na aba Custom. \
  Fig2 - Code component \
![CODE COMPONENT](https://github.com/icaroafoliveira/PsychopyTrigger/blob/main/tela2(CODE_COMPONENT).png)

## Passo 2) CODE PROPERTIES:
Fig3 - Code Properties
![Psychopy Code Properties](https://github.com/icaroafoliveira/PsychopyTrigger/blob/main/tela3.png)

Escreva o código abaixo nas abas correspondentes. O código irá esperar o recebimento do sinal do scanner para iniciar o experimento.\
No meu caso, o scanner envia um sinal TTL (5V), e um arduino nano recebe e converte esse sinal. Na conversão do sinal, codifiquei o arduíno para reportar um 't'.

### Na aba **Begin Experiment**
```
import serial
ser = serial.Serial('COM4', 9600, timeout = 1)
```
### Na aba **Each Frame**
```
if ser: # Only try to read if the serial port was opened successfully
    if ser.in_waiting > 0: # Check if there's data in the serial buffer
        incoming_byte = ser.read().decode().strip() # Read one byte and decode

        # Check if the received character is your trigger character
        if incoming_byte == 't': # <<< YOUR TRIGGER CHARACTER 't'
            continueRoutine = False # End the current routine (WaitForScanner)
            # You can also record the trigger time here if needed
            # trigger_time = globalClock.getTime()
            print(f"Trigger 't' received at {globalClock.getTime()}!")
        else:
            print(f"Received unexpected: {incoming_byte}")
            received_data.append(incoming_byte) # For debugging
```
### Na aba **End Experiment**

```
if ser:
    ser.close()
    print(f"Serial port {arduino_port} closed.")
```

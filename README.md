# PsychopyTrigger
Esse é o passo-a-passo para o setup do trigger.

## Passo 1) Utilizando Psychopy Builder
Fig1 - Tela inicial
![Tela](C:/Users/Stimulus/Documents/github/tela1.png)
- Crie um texto informando o paciente/participante que o experimento irá iniciar em instantes.
    Exemplo:"O experimento irá se iniciar em alguns instantes"
- Em seguida crie um componente para código (CODE component) na aba Custom.
  Fig2 - Code component
## Passo 2) Dentro do CODE:
Fig3
Crie um código em python para abrir a porta serial, e no momemto que a porta recebe o sinal TTL, o experimento se inicia.

### Na aba **Begin Experiment**
```
imnport serial
ser = serial.Serial('COM4', 9600, timeout = 1)>
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

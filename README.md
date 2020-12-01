# Proyecto4_B67709

#### 4.1. Modulación QPSK

##### En la primera parte, se trabajó utilizando como base la sección 3.2., en la que se utilizaba más bien un estándar BPSK. Se hicieron cambios en las funciones, utilizando la suma positiva y negativa de un coseno y un seno. Posteriormente, se realiza la suma de las señales y de las portadorras, para obtener así la salida de estas.

    # import numpy as np

    # 0.1. Definir función 
    def modulador(bits, fc, mpp):
   
        # 1. Parámetros de la 'señal' de información (bits)
        N = len(bits) # Cantidad de bits

        # 2. Construyendo un periodo de la señal portadora c(t)
        Tc = 1 / fc  # periodo [s]
        t_periodo = np.linspace(0, Tc, mpp)
        portadora = np.sin(2*np.pi*fc*t_periodo)
        portadora_ = np.cos(2*np.pi*fc*t_periodo)

        # 3. Inicializar la señal modulada s(t)
        t_simulacion = np.linspace(0, N*Tc, N*mpp) 
        senal_Tx = np.zeros(t_simulacion.shape)
        senal_Tx_ = np.zeros(t_simulacion.shape)
        moduladora = np.zeros(t_simulacion.shape)  # señal de información
 
        # 4. Asignar las formas de onda según los bits (BPSK)
        for i in range(0, N, 2):
            tam = bits[i]
        
            if tam == 1:
                senal_Tx[ i*mpp : (i+1)*mpp] = portadora
            else:
                senal_Tx[i*mpp : (i+1)*mpp] = (portadora * -1)
    
        for i in range(1, N, 2):
            tam_ = bits[i]
            
            if tam_ == 1:
                senal_Tx_[ i*mpp : (i+1)*mpp ] = portadora_
            
            else: 
                senal_Tx_[i*mpp : (i+1)*mpp] = (portadora_ * -1)
    
        Rsenal_Tx = senal_Tx + senal_Tx_
        Rportadora = portadora + portadora_
            
    
        # 5. Calcular la potencia promedio de la señal modulada
        Pm = (1 / (N*Tc)) * np.trapz(pow(senal_Tx, 2), t_simulacion)
    
        return senal_Tx, Pm, portadora

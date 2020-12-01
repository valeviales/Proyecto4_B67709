# Proyecto4_B67709

#### 4.1. Modulación QPSK

##### En la primera parte, se trabajó utilizando como base la sección 3.2., en la que se utilizaba más bien un estándar BPSK. Se hicieron cambios en las funciones, utilizando la suma positiva y negativa de un coseno y un seno. Posteriormente, se realiza la suma de las señales y de las portadorras, para obtener así la salida de estas. La segunda parte de esta sección se prosiguió de manera muy similar al ejemplo.

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

#### 4.2. Estacionaridad y ergodicidad 

##### En esta sección, se buscaron todas las posibles soluciones correspondientes a la función utilizada. Esto se logró usando un ciclo for. Posteriormente se obtuvo el promedio. Se obtuvieron los valores teóricos y reales, así como la gráfica. 

        
        import numpy as np
        import matplotlib.pyplot as plt
        import time

        #Se genera arreglo 

        array = np.linspace(0, 10, 100)

        l = [1, -1]

        s_a = np.empty((4, len(array)))

        # Se calculan las funciones

        for i in l:
            s1 = i*(np.sin(2*np.pi*fc*array) + np.cos(2*np.pi*fc*array))
            s2 = i*(np.sin(2*np.pi*fc*array) - np.cos(2*np.pi*fc*array))
    
            s_a[i,:] = s1
            s_a[i+1,:] = s2
            plt.plot(array, s1)
            plt.plot(array, s2)

        #Cálculo de la media y el valor esperado de la señal

        for i in range(len(array)):
            np.mean(s_a[:,i])

            plt.plot(array, A, lw = 6, color='r', label = "Promedio real")

        E = np.mean(Rsenal_Tx)*array
        plt.plot(array, E, '-', lw = 2, color = 'b', label = "Promedio teorico")

        plt.title('Estacionaridad y ergocidad')
        plt.ylabel("s(t)")
        plt.xlabel("t")
        plt.legend()
        
#### 4.3. Densidad espectral de potencia

##### En esta sección, se obtuvo el valor de la transformada de fourier de la señal. Posteriormente se encontraron los valores del tiempo de la simulación, y se generó la gráfica utilizando linspace.

        from scipy import fft

        # Se obtiene la transformada de fourier
        sig = fft(Rsenal_Tx)

        n_m = len(Rsenal_Tx)

        # Tiempo que tarda la simulación en llevarse a cabo
        t = (n_m//mpp)*(1/fc)

        f = np.linspace(0.0, 1.0/(2.0*((1/fc)/mpp)), n_m//2)


        plt.plot(f,3/n_m*np.power(np.abs(sig[0:n_m//2]),2))
        plt.xlim(0,10000)
        plt.grid()
        plt.show()
  

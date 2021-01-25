# Solidity 101


## Contracto Inteligente

Para definir un contrato inteligente se usa la palabra reservada `contract`

```solidity
contract Ownable {
    constructor() public {

    }
}
```

## Métodos 
````

contract MyContrato {

    string private mensaje;

    function establecerMensaje(string nuevoMensaje) public{
        mensaje = nuevoMensaje;
    }

}
````


## Modificadores de acceso

* public 
* private 
* internal (equivalente a protected)


> La palabra reservada `view` establece si el método no ejecutara ninguna transacción. 


## Modificadores personalizados

Dentro de un contrato hacemos el siguiente modificaor personalizado de la siguiente forma.


```
contract MyContrato {

    string private mensaje;
    address internal owner;


    // Modificador que valida que sea solo el propietario
    modifier isOwner(){
        require(owner == msg.sender); // Condicion
        _;
    }

    // Solo el propietario prodra ejecutar la funcion
    function establecerMensaje(string nuevoMensaje) public isOwner{
        mensaje = nuevoMensaje;
    }

    // msg es un objeto global

}

```

## Herencia

`ContratoPadre.sol`

````
contract ContratoPadre {

}
````

`ContratoHijo.sol`

````
import "./ContratoPadre.sol";

contract ContratoHijo is ContratoPadre {

}
````


## Métodos pagables
Son métodos que requieren cierta cantidad de divisa para poder ejecutarse. Para ello se usa la palabra reservada ``paylable` en el método.



````

contract ContratoConPago {

    string private mensaje;


    function cambiarMensaje(string nuevoMensaje) public payable {
        /* 
          esta línea valida primero si el que llama al método tiene exactamente 
          3 ethers para poder ejecutar el método
        */  
        require(msg.value == 3 ether);
        mensaje = nuevoMensaje;
    }

}
````


## Obtener el balance de un contrato



````

contract MyContrato {

    function obtenerBalance() public view returns (uint) {
        return address(this).balance;
    }

}
````


## Transferir divisas

En el siguiente fragmento se muestra un ejemplo de como transferir
divisas a la cuenta del propietario del contrato como a otra cuenta.

````

contract MyContrato {

    address private owner;

    // Transferir una  al propietario
    function transfer(uint amount ) public isOwner{
         
        /*
          Se valida que el monto solicitado sea igual o menor que el balance del contrato
        */

        require(address(this).balance >= amount);
        owner.transfer(amount);
    }
    
    // Transferir a una determinada cuenta
    function transferTo(uint amount, address to) public isOwner{
        require(address(this).balance >= amount);
        // Se valida que la cuenta destino sea válida
        require(to != address(0));
        to.transfer(amount);
    }


    // Modificador que valida que sea solo el propietario
    modifier isOwner(){
        require(owner == msg.sender); // Condicion
        _;
    }

}
````
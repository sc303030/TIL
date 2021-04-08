# [생활코딩] JAVA1 - 10. 디버거

- 디버거를 실행하면 한 줄 한줄 씩 실행할 수 있다.

- 또한 어떻게 생겼는지 알 수 있다.

```java
import org.opentutorials.iot.Elevator;
import org.opentutorials.iot.Lighting;
import org.opentutorials.iot.Security;

public class OkJavaGoInHome {

	public static void main(String[] args) {
		
		String id = "JAVA APT 507";
		
		// Elevator call 
		Elevator myElevator = new Elevator(id);
		myElevator.callForUp(1);
		
		// Security off 
		Security mySecurity = new Security(id);
		mySecurity.off();
		
		// Light on
		Lighting hallLamp = new Lighting(id+" / Hall Lamp");
		hallLamp.on();
		
		Lighting floorLamp = new Lighting(id+" / floorLamp");
		floorLamp.on();

	}

}
```

- 이렇게 코딩한 것을

```java
package org.opentutorials.iot;

import java.util.Random;

public class Lighting implements OnOff{
	String _id;
	public Lighting(String id){
		this._id = id;
	}
	public boolean on() {
		System.out.println(this._id + " /  Lighting on");
		return true;
	}
	public boolean off() {
		System.out.println(this._id + " /  Lighting off");
		return true;
	}
	public Boolean isOn() {
		Random rand = new Random();
		return rand.nextBoolean();
	}
	
}
```

- 이런식으로 볼 수 있다.
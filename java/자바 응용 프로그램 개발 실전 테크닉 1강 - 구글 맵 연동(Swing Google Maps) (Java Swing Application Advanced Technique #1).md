# 자바 응용 프로그램 개발 실전 테크닉 1강 - 구글 맵 연동(Swing Google Maps) (Java Swing Application Advanced Technique #1)

```java
public class Execute {

	public static void main(String[] args) {
		
		Main main = new Main();

	}

}
```

```java
import javax.swing.JFrame;

public class Main extends JFrame{
	
	public Main() {
		setTitle("Google Maps");
		setVisible(true);
		pack(); 
	}

}
```

```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.URL;
import java.net.URLEncoder;

import javax.swing.ImageIcon;

public class GoodleAPI {
	
	public void downloadMap(String location){
		try {
			String imageURL =  "https://developers.google.com/maps/api/staticmap?center="
								+URLEncoder.encode(location, "UTF-8") + "&zoom=11&size=612x612&scale=2";
			URL url = new URL(imageURL);
			InputStream is = url.openStream();
			OutputStream os = new FileOutputStream(location);
			byte[] b = new byte[2048];
			int length;
			while ((length = is.read(b)) != -1) {
				os.write(b,0,length);
			}
			is.close();
			os.close();
		}catch(Exception e) {
			e.printStackTrace();
		}
		
	}
	public ImageIcon getMap(String location) {
		return new ImageIcon((new ImageIcon(location)).getImage().getScaledInstance(612, 612, java.awt.Image.SCALE_SMOOTH));
	}
	public void fileDelete(String fileName) {
		File f = new File(fileName);
		f.delete();
	}
}
```

- URLEncoder.encode
  - 인코딩이 다르면 안되니깐 utf-8로 맞춰준다.

```java
import javax.swing.JFrame;
import javax.swing.JLabel;

public class Main extends JFrame{
	
	private GoogleAPI googleAPI = new GoogleAPI();
	private String location = "서울";
	private JLabel googleMap;
	
	public Main() {
		googleAPI.downloadMap(location);
		googleMap = new JLabel(googleAPI.getMap(location));
		googleAPI.fileDelete(location);
		
		setTitle("Google Maps");
		setVisible(true);
		pack(); 
	}

}
```

- 현재 api는 다른 주소로 바꿔서 해야한다. 구글맵에서 따로 api키를 받아서 입력하는 방식으로 변했다.
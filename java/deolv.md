## csv beolvasás

Arraylist el olvassuk ki 
ezért ltrehozun egy változót 

pl. 
    ArrayList<Employee> emplist;
    ArrayList<Employee> birthlist;

majd létrehozunk egy fügvényt

értéket nem adunk vissza ezért void
beolvasás során elkel dobnunk az Exception

public void beolvas() throws FileNotFoundException {

//egy File al adduj meg a beolvasni való file nevét ami az emp.csv
File file = new File("epm.csv");
//scanner segitségitségével beállitjuk a karakter kódolást ami utf-8 és meg adjuk hogy minek 
Scanner sca = new Scanner(file, "utf-8");
//nextline al egy sort kihagyunk ha az adatok megnevezése van megadva elsőnak
sca.nextLine();

    while(sca.hasNextLine()){
        String line = sca.nextline();
     //line.split al moondjuk meg hogy mi választja el a beolvasni kívánt adatokat
     String[] lineArray = line.split(",");
     Systam.out.println(line);
     //itt adjuk meg a dolgozók adatait és mondjuk meg hogy milyen formátumban vannak az Employee.java alapján (lent folyt)
     Employee emp = new Employee(Integer.parseInt(lineArray[0]), lineArray[1], lineArray[2], Double.parseDouble(lineArray[3]), LocalDate.parse(lineArray[4]));
    }
    Systam.out.println("");
    //lezárjuk a scannert
    sca.close();

}

//ne felejtsuk el a main fugvényme inicializálni a létrehozott ArrayList<Employee> emplist et és a további fugvényeket megadni
pl.
 public MainConsole() throws Exception{

    emplist = new ArrayList<>();
    birthlist = new ArrayList<>();
    beolvas();
    feladat1();
    feladat2();
    }

//Létrehozunk egy Employee.java file t
//itt megadjuk a dolgozók adatait és formátumukat
public class Employee {
    //külön változokban megadjuk őket
    //LocalDate importánli kell a java.time.LocalDate csomagból
    Integer id;
    String name;
    String city;
    Double salary;
    LocalDate birthdate;

    //fügvénynek megadjuk az értékeket
    public Employee(
        int id, 
        String name, 
        String city, 
        double salary, 
        LocalDate birthdate) {
            
    this.id = id;
    this.name = name;
    this.city = city;
    this.salary = salary;
    this.birthdate = birthdate;
}

## consol kiíratás

    public void feladat1(){
    //for nak megadjuk az Employee t amit emp ként adunk át 
      for(Employee emp: this.emplist){
        //kiíratjuk a dolhozó nevét ha megvelel az if ben megadott feltételeknek
         System.out.println(emp.name);
         //getYear a LocalDate comagból használjuk
         if(emp.birthdate.getYear() == 1982 ) {
            this.birthlist.add(emp);
         }
      }
      System.out.println("");

   }

## külön file ba írás

létrehozuk egy új fügvényt

 public void feladat2() throws FileNotFoundException{
    //PrintWriter el tudunk új filt létrehozni és abba beleírni amit importálni kell a java.io.PrintWriter csomagból
    //megadjuk a file nak a nevét amit létrehozzon és beleírjon
      PrintWriter pw = new PrintWriter("kiie.csv");
    
      for(Employee emp: birthlist ){
         pw.append(emp.id.toString());
         pw.append(",");
         pw.append(emp.name);
         pw.append(",");
         pw.append(emp.city);
         pw.append(",");
         pw.append(emp.salary.toString());
         pw.append(",");
         pw.append(emp.birthdate.toString());
         pw.append(",");
      }
      //mindenkép le kell zárni
      pw.close();
   }
## backend

laravel project generálás
composer create-project laravel/laravel 

server indítás
php artisan serve

controller generálás
php artisan make:controller "név"

Request generálás
php artisan make:request StoreStudentData

migration generálás
php artisan make:migration create_students_table

tábla generálása
php artisan migrate 

Model generálás
php artisan make:model 

seeder generálás
php artisan make:seeder ShipSeeder

adatok feltöltése seederrel
php artisan db:seed --class="seeder_neve"

.env file ba tudjuk megadni az adatbázis nevét
DB_DATABASE="name"

vendor mappa létrehozása
composer install


### api.php

//apiba importáljuk ba az adott controller amit használni szeretnénk pl.
use App\Http\Controllers\ShipController;

//itt adjuk meg a Route t nak hogy milyen metódust használjon és milyen útvonalon
//meg adjuk hogy melyik controllert használja és hogy melyik fügvényt
// metodusok lehetnek get, post, put, delete pl.
Route::get('/ships',[ShipController::class,"index"]);
Route::post('/ships',[ShipController::class,"store"]);
Route::put('/ships/{id}',[ShipController::class,"update"]);
Route::delete('/ships/{id}',[ShipController::class,"destroy"]);

### controller.php

//importálni kell a megadott modelt amit használni akarunk pl.
use App\Models\ship;
use Illuminate\Http\Request;

//fügvény készítése 
puvlic function name(){

}

//érték megadása fügvényen belül pl. 
// $ jellel adjuk meg az értékeket
public function index(){
    $ships = Ship::all();
    return response()->json(['data' => $ships], 201);
}

//fügvénynek tudunk értéket is mefadni pl.
public function store(Request $request){
    $input = $request->all();
    $ship = Ship::create($input);
    return response()->json(['data' => $ship], 201);
}

// request en több mindent tudunk futtatni lehet az all, find, destroy

//id is át tudunk addni fugvénynek értékként pl.
//visszatérést kötelező írni ami json formába tér vissza
//meg kell addnunk mit akarunk vissaz addni "data" ami az adatunk és hogy mi  az ami a "$ship" és egy hiba kódot
public function update(Request $request, $id){
   $ship = Ship::find($id);
   $ship->update($request->all());
   return response()->json(['data' => $ship], 201);
}

//egy értéket is átadhatunk a fügvénynek pl. az "$id"
public function destroy($id){
  $ship = Ship::destroy($id);
  return response()->json(['data' => $ship], 201);
}

### model

//itt addjuk meg hogy milyen adatokat akarunk áradni pl.
//ezt lrdemes protected be tenni
protected $fillable = [
        'name',
        'length',
        'price',
        'person',
        'trailer'
    ];

### migration

//itt tudjuk megadni hogy a táblában az adat mileny tipusu pl.
//lehet string, integer, double, boolean vagy foreignId az az idegen kulcs
//ahova akarjuk az idegen kulcsot abba a táblába kell beletenni a másik tábla id nevével
//id kötelezü megadni
Schema::create('ships', function (Blueprint $table) {
            $table->id();
            $table->string("name");
            $table->integer("length");
            $table->double("price");
            $table->integer("person");
            $table->boolean("trailer");
            $table->timestamps();
        });

//idegen kulcs esetén a modelbe kell megadnunk hogy mi mihez arányos 
//pl. ha több színünk van de több hajo is lehet ugyan olyan színü
//hajó model
public function color(){
        return $this -> belongsTo(Color::class);
    }
//szíin modell
public function ship(){
        return $this -> hasMany(Ship::class);
    }

//ilyenkor érdemes kiszedni a timestamps  et 
public $timestamps = false;

//migrálásnál érdemes az idegen kulcsot tartalmazó táblát migrálni másodjára a mi esetünkben az a ship. ehez elég átnevezni a migrálni kíván file t

## seeder

//seeder segítségével fel tudjuk tölteni a táblánkat pl.
DB::table("ships")->insert([
            "name" => "Maki Verem",
            "length" => 120,
            "price" => 12345,
            "person" => 2,
            "trailer" => 1,
        ],
        [
            következő
        ]);


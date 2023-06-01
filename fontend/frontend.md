## angular

angular project generálás
ng new project-name

componsens generálás
ng g c componens-name

service generálás 
ng generate service service-name vagy
ng generate service /folder-name/service-name

bootstrap hozzáadás
ng add bootstrap
styles.scss @import "bootstrao";

bootstrap útvonal angular.json
"scripts": [
              "node_modules/bootstrap/dist/js/bootstrap.js"
            ]

szerver indítás 
ng serve vagy ng serve -o

## root
//ha a weboldalon root ot szeretnénk használni akkor
//természere
<app-root></app-root>


### modulok
//ha bármilyen modulet hozzá szeretnénk addni azt a app.module.ts be kell megadni pl.

HttpClientModule,
ReactiveFormsModule
//utánna importálni pl.

import {HttpClientModule} from '@angular/common/http';
import { ReactiveFormsModule } from '@angular/forms';

### táblázat

//táblázat létrehozása
<table>
//táblázat fejléc
<thead>
<th>fejlécen belül "th" vannak </th>
</thead>
//táblázat teste
<tbody>
    <tr>
    <td>felsorolás</td>
    </tr>
</tbody>
</table>

//táblázatba szeretnént adatokat megjeleníteni azt al alábbi módon tehetjük meg
<tr *ngFor="let product of products">
        <td>{{product.name}}</td>
        <td>{{product.salary}}</td>
</tr>

### modals

//add modalt akarunk létrehozni formGroup segítségével
<div class="modal fade" id="addModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h1 class="modal-title fs-5" id="exampleModalLabel">Add Product</h1>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>

    //figyeljünk arra hogy a nevek megegyezenek a label for, id , formControlName="name"
    //type ba megadhatjuk hogy milyen tipust szeretnénk pl. text, number

      <div class="modal-body">
        <form [formGroup]="productForm">
          <div class="mb-3">
            <label for="name" class="form-label">Name</label>
            <input type="text" class="form-control" id="name"
            placeholder="expl:  g250" formControlName="name" >
          </div>

          <div class="mb-3">
            <label for="length" class="form-label">Length</label>
            <input type="number" class="form-control" id="length"
            placeholder="expl:  13" formControlName="length">
          </div>
        </form>
      </div>

    //module on belül gomb
    //uygan úgy kell megadni ha fügvényre hivatkozunk

<div class="modal-footer">
        <button type="button" class="btn btn-secondary" 
        data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary"
        data-bs-dismiss="modal"
        (click)="onClick()">Save</button>
      </div>
    </div>
  </div>
</div>

//ha egy táblázatba szeretnénk gombal modalt eöhívni hozzáadást
//törrlés gombnál a fügvényben adjunk át értéket álltalában id t
<div class="container">
  <button class="btn btn-warning my-3" data-bs-toggle="modal" data-bs-target="#addModal">Add</button>
  <table>
    <thead>
      <th>Név</th>
    </thead>
    <tbody>
      <tr *ngFor="let product of products">
        <td>{{product.name}}</td>
        <td>
          <div class="row mx-2">
            <button class="text-dark bg-warning p-2 rounded border-warning" (click)="editProduct(ship)" data-bs-toggle="modal" data-bs-target="#modifyModal" >
              Edit 
          </button>
          </div>
        </td>
        <td>
          <button (click)="deleteShip(ship.id)">Törlés</button>
        </td>
      </tr>
    </tbody>
  </table>
</div>



### gomb létrehozása 

<button (click)="fügvényneve(értéke)">szöveg</button>


### component.ts

//importálni kell a megadott modult amut szeretnénk használni pl.
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { ApiService } from '../api.service';

//formgroup kezelése 
//külső értéknem felveszük groupfrom ot
//dolgozók kezelése shipss: any vel 
empForm!: FormGroup;
  editForm!: FormGroup;
  ships: any;

//constructor nak megadjuk hogy milyen importokat használunk pl.
  constructor(private api: ApiService, private formBuilder: FormBuilder){}

   ngOnInit(): void {
    this.productForm = this.formBuilder.group({
      name: ['', Validators.required],
      length: [''],
    });
//edited id alapján hajtjuk végre
    this.editForm = this.formBuilder.group({
      id: [''],
      name: ['', Validators.required],
      length: [''],
    });
//fügvény meghívása
    this.getProducts();
  }
//fügvény lekéri a restapi ból az adatbázisban lévő hajókat
//subscribe() al tudjuk lekérni
  getProducts(){
    this.api.getProducts().subscribe(
      res => {
        console.log(res.data);
        this.ships = res.data;
      }
    );
  }

//hozzáadás
//megadjuk hogy mit szeretnénk hozzáaddni milyen értékhaz
    addProducts() {
    let data = {
      name: this.productForm.value.name,
      length: this.productForm.value.length,
    };
//fenti data ba tároljuk el az új hajó adatait
//addProduct(data) adjuk hozzá 
    this.clearField();
    this.api.addProduct(data).subscribe({
      next: (data: any) => {
        console.log('vissza: ' + data);
        this.getProducts();
      },
      error: (err: any) => {
      }
    });
  }

//
  clearField() {
    this.productForm.patchValue({
      name: '',
      length: '',
    });
  }
//törlés nél a törölni kívánt hajó id ját adjuk át
//majd visszatérünk a getShips()
  deleteShip(id: number){
    console.log(id);
    this.api.deleteProduct(id).subscribe({
      next: (res:any) => {
        this.getProducts();
      },
      error: (err) => {
      }
    });
  }

// editnél átadjuk a "product: any" ot 
//editForm ot használva megadjuk patchValue ba hogy mit szeretnénk editelni
    editProduct(product: any) {
    this.editForm.patchValue({ id: product.id });
    this.editForm.patchValue({ name: product.name });
    this.editForm.patchValue({ length: product.length});
  }
//update nél 
  updateProduct() {
    let data = {
      id: this.editForm.value.id,
      name: this.editForm.value.name,
      length: this.editForm.value.length,
    };

    this.api.updateShip(data).subscribe({
      next: (res: any) => {
        console.log(res.message);
        this.getShips();
      },
      error: (err) => {
      }
    });
    }

### api.service.ts

//ügyeljünk a fügvénynevek egyezésére a component.ts el

//importáljuk a használni kívánt modulokat pl.
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Injectable } from '@angular/core';

//constructor nak megadjuk a használni kívánt modult pl.
constructor(private http: HttpClient) { }

//restapi csatlakozás 
//külön értékként megadunk egy url t
 url = "http://localhost:8000/api/valami";

 //itt csak megjelenítünk ezét csak visszatérünk mindegyik értékkel get metódust használva
  getProducts(){
    return this.http.get<any>(this.url);
  }

//
  addProduct(product: any){
    let httpHeaders = new HttpHeaders();
    httpHeaders.append("Content-Type","application/json");
    let httpOptions = {
      headers: httpHeaders
    };
    return this.http.post(this.url,ship,httpOptions)
  }

//megadjuk url elérést a törléshaz id alapján
//deleteShip(id: number) megadjuk hogy milyen érték alalpján töröljünk
//visszatérésnél adjuk meg hogy milyen metódust hajtson végre és  törlésnél delete
  deleteShip(id: number){
    let fullurl = this.url + '/' + id;

    return this.http.delete(fullurl);
  }

//átadjuk az url t és az id val mondjuk meg hogy mit szeretnénk update elni
//updateShip(ship: any) fügvénynek értéket adunk "ship: any"
// visszatérés put metódussal történik értéknek megadjuk a fullurl t és hogy mivel "ship", httpOptions
  updateShip(ship: any){
    let fullurl = this.url + '/' + ship.id;
    const headers = new HttpHeaders({
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    });

    const httpOptions = {
      headers: headers,
    };

    return this.http.put(fullurl,ship, httpOptions);
  }


# FullStack-Developer-Assignment
<!-- trade-form.component.html -->
<form>
  <label for="tradeDateTime">Trade Date & Time:</label>
  <input type="datetime-local" id="tradeDateTime" name="tradeDateTime" [(ngModel)]="trade.tradeDateTime" required>

  <label for="stockName">Stock Name:</label>
  <input type="text" id="stockName" name="stockName" [(ngModel)]="trade.stockName" required>

  <label for="listingPrice">Listing Price:</label>
  <input type="number" id="listingPrice" name="listingPrice" [(ngModel)]="trade.listingPrice" required>

  <label for="quantity">Quantity:</label>
  <input type="number" id="quantity" name="quantity" [(ngModel)]="trade.quantity" required>

  <label for="type">Type:</label>
  <select id="type" name="type" [(ngModel)]="trade.type" required>
    <option value="buy">Buy</option>
    <option value="sell">Sell</option>
  </select>

  <label for="pricePerUnit">Price Per Unit:</label>
  <input type="number" id="pricePerUnit" name="pricePerUnit" [(ngModel)]="trade.pricePerUnit" required>

  <button type="submit" (click)="addTrade()">Add Trade</button>
</form>

// trade-form.component.ts
import { Component } from '@angular/core';
import { Trade } from './trade.model';
import { TradeService } from './trade.service';

@Component({
  selector: 'app-trade-form',
  templateUrl: './trade-form.component.html',
  styleUrls: ['./trade-form.component.css']
})
export class TradeFormComponent {
  trade: Trade = new Trade();

  constructor(private tradeService: TradeService) {}

  addTrade() {
    this.tradeService.addTrade(this.trade)
      .subscribe(() => {
        console.log('Trade added successfully');
        // Add logic to reset form or handle success
      });
  }
}
// trade.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Trade } from './trade.model';

@Injectable({
  providedIn: 'root'
})
export class TradeService {
  private apiUrl = 'http://localhost:8080/api/trade-details';

  constructor(private http: HttpClient) {}

  addTrade(trade: Trade): Observable<Trade> {
    return this.http.post<Trade>(this.apiUrl, trade);
  }
}
// trade.model.ts
export class Trade {
  tradeDateTime: string;
  stockName: string;
  listingPrice: number;
  quantity: number;
  type: string;
  pricePerUnit: number;
}

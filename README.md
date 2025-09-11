## Hospital Management System
- Created a hospital management system to streamline hospital operations such as patient management, appointments, and staff administration using the MERN stack.
- Implemented JWT-based authentication with multi-token management for secure and efficient session handling.
- Designed a scalable architecture with dual frontends for users and admins, ensuring role-based access control and a user-friendly React.js interface.
```
// src/OrdersBootstrap.js
import React, { useState } from "react";

export default function OrdersBootstrap({ apiUrl = "http://localhost:8080/api/orders" }) {
  const [orders, setOrders] = useState([]);
  const [visible, setVisible] = useState(false);
  const [error, setError] = useState(null);

  async function handleDisplayClick() {
    setError(null);
    try {
      const res = await fetch(apiUrl, { method: "GET", headers: { "Accept": "application/json" } });
      if (!res.ok) throw new Error(`Server returned ${res.status}`);
      const data = await res.json();
      setOrders(Array.isArray(data) ? data : []);
      setVisible(true); // show table after data is fetched
    } catch (err) {
      setOrders([]);
      setVisible(false);
      setError(err.message || "Failed to load orders");
    }
  }

  return (
    <div className="container my-4">
      <div className="d-flex justify-content-between align-items-center mb-3">
        <h3>Orders</h3>
        <button className="btn btn-primary" onClick={handleDisplayClick}>
          Display Orders
        </button>
      </div>

      {error && (
        <div className="alert alert-danger" role="alert">
          Error: {error}
        </div>
      )}

      {visible && (
        <div className="table-responsive">
          <table className="table table-striped table-bordered">
            <thead className="table-light">
              <tr>
                <th style={{ width: 80 }}>#</th>
                <th>Order Name</th>
              </tr>
            </thead>
            <tbody>
              {orders.length === 0 ? (
                <tr>
                  <td colSpan="2" className="text-center">No orders found.</td>
                </tr>
              ) : (
                orders.map((o) => (
                  <tr key={o.orderId ?? o.id}>
                    <td>{o.orderId ?? o.id}</td>
                    <td>{o.orderName}</td>
                  </tr>
                ))
              )}
            </tbody>
          </table>
        </div>
      )}
    </div>
  );
}
```

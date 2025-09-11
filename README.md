## Hospital Management System
- Created a hospital management system to streamline hospital operations such as patient management, appointments, and staff administration using the MERN stack.
- Implemented JWT-based authentication with multi-token management for secure and efficient session handling.
- Designed a scalable architecture with dual frontends for users and admins, ensuring role-based access control and a user-friendly React.js interface.
```
import React, { useState } from "react";
import React, { useState } from "react";

export default function CustomersTableButton({ apiUrl = "http://localhost:8080/api/customers" }) {
  const [customers, setCustomers] = useState([]);
  const [error, setError] = useState(null);
  const [visible, setVisible] = useState(false);

  async function handleFetchCustomers() {
    setError(null);
    try {
      const res = await fetch(apiUrl);
      if (!res.ok) throw new Error(`Server returned ${res.status}`);
      const data = await res.json();
      setCustomers(Array.isArray(data) ? data : []);
      setVisible(true); // show table only after data is fetched
    } catch (err) {
      setError(err.message || "Failed to load customers");
      setCustomers([]);
      setVisible(false);
    }
  }

  return (
    <div className="container my-4">
      <h3>Customers</h3>

      <div className="mb-3">
        <button className="btn btn-primary" onClick={handleFetchCustomers}>
          Display Customers
        </button>
      </div>

      {error && <div className="alert alert-danger">Error: {error}</div>}

      {visible && (
        <div className="table-responsive">
          <table className="table table-striped table-bordered">
            <thead className="table-light">
              <tr>
                <th style={{ width: 80 }}>#</th>
                <th>Name</th>
                <th>Email</th>
                <th>Orders</th>
              </tr>
            </thead>
            <tbody>
              {customers.length === 0 ? (
                <tr>
                  <td colSpan="4" className="text-center">No customers found.</td>
                </tr>
              ) : (
                customers.map((c) => (
                  <tr key={c.id}>
                    <td>{c.id}</td>
                    <td>{c.name}</td>
                    <td>{c.email}</td>
                    <td>
                      {Array.isArray(c.orders) && c.orders.length > 0 ? (
                        c.orders.map(o => o.orderName).join(", ")
                      ) : (
                        <span className="text-muted">—</span>
                      )}
                    </td>
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

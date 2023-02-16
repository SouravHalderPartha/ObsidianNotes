```Typescript
 ApproveSalesOrderFromSalesOrder(data, whStatusId): Observable<any> {

    return this.http

      .post<any>(

        this.baseurl +

          `SalesOrder/ApproveSalesOrderFromSalesOrder/?whStatusId=${whStatusId}`,

        data

      )

      .pipe(retry(1));

  }
```

```c#
[HttpPost("ApproveSalesOrderFromSalesOrder")]
        public async Task<IActionResult> ApproveSalesOrderFromSalesOrder([FromBody] SalesOrder entity, int whStatusId)
        {
            try
            {
                
                if (entity == null)
                    return Ok(new { Success = false, Message = "check order details" });

                var now = DateTime.UtcNow;

                if (entity.Id == 0)
                {
                    entity.CreatedAt = DateTime.UtcNow;
                    entity.CreatedById = this.UserId;
                }
                else
                {
                    entity.UpdatedAt = DateTime.UtcNow;
                    entity.UpdatedById = this.UserId;
                }
                entity.StatusId = (int)SalesOrderStatus.OnGoing;
                entity.CompanyId = this.CompanyId;
                var response = await _saleOrderRepository.SaveSalesOrderApprove(entity, Convert.ToInt32(whStatusId));
                return Ok(response);
            }
            catch (Exception ex)
            {
                Logger.Error("Problem happened in server " + ex);
                return BadRequest("Ops something wrong, please contact support team");
            }
        }
```

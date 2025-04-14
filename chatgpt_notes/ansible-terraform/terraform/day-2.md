✅ **Terraform Blocks**  
- `resource`: Defines infrastructure (e.g., EC2, S3)  
- `variable`: Declares input variables  
- `output`: Displays results after apply  
- `locals`: Sets local variables  
- `data`: Fetches external data (e.g., AMIs)  
- `null_resource`: Logic-only resource, triggers with `triggers`  
- `terraform`: Configures backend, provider versions, etc.  

---

✅ **Essential Commands**  
- `terraform init`: Initialize working directory  
- `terraform plan`: Preview changes  
- `terraform apply`: Apply changes  
- `terraform destroy`: Delete infrastructure  
- `terraform fmt`: Format code  
- `terraform validate`: Check syntax & internal logic  

---

🔁 **Variable Practice**  
- `terraform.tfvars`  
- `*.auto.tfvars`  
→ Auto-loaded for variable values  

---
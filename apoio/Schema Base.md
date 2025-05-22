# Schema Base
O schema base lorem ipsum...

### Schema atual no NavegaLibs
```
{
  name: { type: String, required: true, maxLength: 50, trim: true },
  isDefault: { type: Boolean, required: true, default: false },
  status: { type: Schema.Types.ObjectId, ref: 'configuration_status', required: true },
}
```
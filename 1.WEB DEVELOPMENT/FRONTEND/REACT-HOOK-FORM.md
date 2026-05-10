
*React hook form is an library which is used to create strong forms with validations easily*
*Its better than from building from scratch since re-renders are fewer in this.*

[DOCUMENTATION](https://react-hook-form.com/get-started) ***READ for further information to use****

## Creating Form:

- We use useForm hook for creating forms from React-hook-form
```jsx
const {
    register,
    handleSubmit,
    watch,
    formState: { errors,isSubmitting },
  } = useForm();
```


**Using it for validation of the forms input make it easier to track and send data to the backend

```jsx
<div>
      <form onSubmit={handleSubmit(onSubmit)}>
        <div>
          <label htmlFor="">first name</label>
          <input
            {...register("firstname", { required: true, minLength: 5 })}
            aria-invalid={errors.firstName ? "true" : "false"}
          />
          {errors.firstName?.type === "required" && (
            <p role="alert">First name is required</p>
          )}
          {errors.firstname && <p>{errors.firstname.message}</p>}
        </div>
        <div>
          <label htmlFor="">middle name</label>
          <input {...register("middlename")} />
        </div>
        <div>
          <label htmlFor="">last name</label>
          <input {...register("lastname")} />
        </div>
        <button type="submit" disabled={isSubmitting}>{isSubmitting?'submitting':'submit'}</button>
      </form>
    </div>
```

**We also support schema-based form validation with [Yup](https://github.com/jquense/yup), [Zod](https://github.com/vriad/zod) , [Superstruct](https://github.com/ianstormtaylor/superstruct) & [Joi](https://github.com/hapijs/joi), where you can pass your `schema` to [useForm](https://react-hook-form.com/docs#useForm) as an optional config. It will validate your input data against the schema and return with either [errors](https://react-hook-form.com/docs#errors) or a valid result.**


---



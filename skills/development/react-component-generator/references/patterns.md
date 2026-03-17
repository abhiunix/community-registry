# Common React Patterns Reference

## Compound Component Pattern

```tsx
function Select({ children, value, onChange }: SelectProps) {
  return (
    <SelectContext.Provider value={{ value, onChange }}>
      <div role="listbox">{children}</div>
    </SelectContext.Provider>
  );
}

Select.Option = function SelectOption({ value, children }: OptionProps) {
  const ctx = useContext(SelectContext);
  return (
    <div
      role="option"
      aria-selected={ctx.value === value}
      onClick={() => ctx.onChange(value)}
    >
      {children}
    </div>
  );
};
```

## Controlled Input Pattern

```tsx
interface InputProps {
  value: string;
  onChange: (value: string) => void;
  label: string;
}

export function Input({ value, onChange, label }: InputProps) {
  const id = useId();
  return (
    <div>
      <label htmlFor={id}>{label}</label>
      <input
        id={id}
        value={value}
        onChange={(e) => onChange(e.target.value)}
      />
    </div>
  );
}
```

## Loading/Error State Pattern

```tsx
interface AsyncContentProps<T> {
  loading: boolean;
  error: string | null;
  data: T | null;
  children: (data: T) => React.ReactNode;
}

export function AsyncContent<T>({ loading, error, data, children }: AsyncContentProps<T>) {
  if (loading) return <Skeleton />;
  if (error) return <ErrorMessage message={error} />;
  if (!data) return null;
  return <>{children(data)}</>;
}
```

# TanStack Table Cheat Sheet

TanStack Table is a powerful headless table library for building tables with React, Vue, Solid, and more.

## Installation

```bash
npm install @tanstack/react-table
```

## The TanStack Table 3-Step Process

Every action in TanStack Table is a 3-step process:
1. **Define/update the column definitions** - Configure how columns behave
2. **Define/update the Table Instance** - Create the table with `useReactTable`
3. **Update the UI** - Render the table using table instance APIs

## Key Concepts

To add any table feature, you need to consider 3 parts:
1. **Column definitions** - Define how each column behaves (accessor, header, cell, sorting, filtering)
2. **Table definition** - Create the table instance with `useReactTable` hook
3. **UI layout** - Render the table using `flexRender` and table instance methods

## Required Items

### Column Definitions

There are 3 types of columns:

1. **Accessor Columns** - Access data directly from row objects
2. **Display Columns** - Custom columns that don't access data directly
3. **Grouping Columns** - Columns that group other columns

#### Basic Column Definition Example

```tsx
import { ColumnDef } from '@tanstack/react-table';

type Person = {
  id: number;
  name: string;
  age: number;
  email: string;
};

const columns: ColumnDef<Person>[] = [
  // Accessor column (most common)
  {
    accessorKey: 'name',
    header: 'Name',
    cell: (info) => info.getValue(),
  },
  // Accessor with custom cell renderer
  {
    accessorKey: 'age',
    header: 'Age',
    cell: (info) => <strong>{info.getValue()}</strong>,
  },
  // Accessor function (for nested data)
  {
    accessorFn: (row) => row.email,
    id: 'email',
    header: 'Email',
  },
  // Display column (no data accessor)
  {
    id: 'actions',
    header: 'Actions',
    cell: (info) => (
      <button onClick={() => handleDelete(info.row.original.id)}>
        Delete
      </button>
    ),
  },
];
```

### Table Definition

```tsx
import {
  useReactTable,
  getCoreRowModel,
  flexRender,
} from '@tanstack/react-table';

const table = useReactTable({
  // Required: column definitions
  columns,
  // Required: data array
  data,
  // Required: core row model for displaying rows
  getCoreRowModel: getCoreRowModel(),
});
```

### UI Layout

```tsx
import { flexRender } from '@tanstack/react-table';

function Table() {
  const table = useReactTable({ columns, data, getCoreRowModel: getCoreRowModel() });

  return (
    <table>
      <thead>
        {table.getHeaderGroups().map(headerGroup => (
          <tr key={headerGroup.id}>
            {headerGroup.headers.map(header => (
              <th key={header.id}>
                {header.isPlaceholder
                  ? null
                  : flexRender(
                      header.column.columnDef.header,
                      header.getContext()
                    )}
              </th>
            ))}
          </tr>
        ))}
      </thead>
      <tbody>
        {table.getRowModel().rows.map(row => (
          <tr key={row.id}>
            {row.getVisibleCells().map(cell => (
              <td key={cell.id}>
                {flexRender(cell.column.columnDef.cell, cell.getContext())}
              </td>
            ))}
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```





## Sorting

### Column Settings

```tsx
{
  accessorKey: 'name',
  header: 'Name',
  // Enable or disable sorting (defaults to true)
  enableSorting: true,
  // Built-in sorting functions
  sortingFn: 
    | 'auto'                    // Automatically detect type
    | 'alphanumeric'            // Case-insensitive string sort
    | 'alphanumericCaseSensitive'
    | 'text'                    // Text sort
    | 'textCaseSensitive'
    | 'datetime'                // Date/time sort
    | 'basic'                   // Simple comparison
    | SortingFn<TData>          // Custom function
}
```

### Table Settings

```tsx
import { useState } from 'react';
import {
  useReactTable,
  getCoreRowModel,
  getSortedRowModel,
  SortingState,
} from '@tanstack/react-table';

function SortableTable() {
  const [sorting, setSorting] = useState<SortingState>([]);

  const table = useReactTable({
    columns,
    data,
    getCoreRowModel: getCoreRowModel(),
    // Enable sorting
    enableSorting: true,
    // Handle sorting state changes
    onSortingChange: setSorting,
    // Get sorted row model
    getSortedRowModel: getSortedRowModel(),
    state: {
      sorting,
    },
  });

  return (
    <table>
      <thead>
        {table.getHeaderGroups().map(headerGroup => (
          <tr key={headerGroup.id}>
            {headerGroup.headers.map(header => (
              <th key={header.id}>
                {header.isPlaceholder ? null : (
                  <div
                    style={{ cursor: header.column.getCanSort() ? 'pointer' : 'default' }}
                    onClick={header.column.getToggleSortingHandler()}
                  >
                    {flexRender(header.column.columnDef.header, header.getContext())}
                    {{
                      asc: ' 🔼',
                      desc: ' 🔽',
                    }[header.column.getIsSorted() as string] ?? null}
                  </div>
                )}
              </th>
            ))}
          </tr>
        ))}
      </thead>
      {/* ... rest of table */}
    </table>
  );
}
```

### Custom Sorting Function

```tsx
const customSortingFn: SortingFn<Person> = (rowA, rowB, columnId) => {
  const a = rowA.getValue(columnId) as string;
  const b = rowB.getValue(columnId) as string;
  // Custom sorting logic
  return a.localeCompare(b);
};

const columns: ColumnDef<Person>[] = [
  {
    accessorKey: 'name',
    header: 'Name',
    sortingFn: customSortingFn,
  },
];
```


## Filtering

### Column Settings

```tsx
{
  accessorKey: 'name',
  header: 'Name',
  // Enable or disable column filtering (defaults to true)
  enableColumnFilter: true,
  // Built-in filter functions
  filterFn: 
    | 'auto'                    // Automatically detect type
    | 'includesString'         // Case-insensitive substring match
    | 'includesStringSensitive'
    | 'equalsString'           // Exact string match
    | 'arrIncludes'            // Array includes value
    | 'arrIncludesAll'         // Array includes all values
    | 'arrIncludesSome'        // Array includes any value
    | 'equals'                 // Deep equality
    | 'weakEquals'             // Weak equality (==)
    | 'inNumberRange'          // Number range filter
    | FilterFn<TData>          // Custom filter function
  // Auto-remove filter when value is falsy
  autoRemove?: (value: any) => boolean;
  // Transform filter value before applying
  resolveFilterValue?: (value: any) => any;
}
```

### Table Settings

```tsx
import { useState } from 'react';
import {
  useReactTable,
  getCoreRowModel,
  getFilteredRowModel,
  ColumnFiltersState,
} from '@tanstack/react-table';

function FilterableTable() {
  const [columnFilters, setColumnFilters] = useState<ColumnFiltersState>([]);

  const table = useReactTable({
    columns,
    data,
    getCoreRowModel: getCoreRowModel(),
    // Enable filtering
    enableColumnFilters: true,
    // Handle filter state changes
    onColumnFiltersChange: setColumnFilters,
    // Get filtered row model
    getFilteredRowModel: getFilteredRowModel(),
    state: {
      columnFilters,
    },
  });

  return (
    <>
      {/* Filter input example */}
      <input
        value={(table.getColumn('name')?.getFilterValue() as string) ?? ''}
        onChange={(e) => table.getColumn('name')?.setFilterValue(e.target.value)}
        placeholder="Filter by name..."
      />
      <table>
        {/* ... table markup */}
      </table>
    </>
  );
}
```

### Global Filtering

```tsx
import { useState } from 'react';
import {
  useReactTable,
  getCoreRowModel,
  getFilteredRowModel,
  getGlobalFilteredRowModel,
} from '@tanstack/react-table';

function GlobalFilterTable() {
  const [globalFilter, setGlobalFilter] = useState('');

  const table = useReactTable({
    columns,
    data,
    getCoreRowModel: getCoreRowModel(),
    // Enable global filtering
    enableGlobalFilter: true,
    // Handle global filter changes
    onGlobalFilterChange: setGlobalFilter,
    // Get globally filtered row model
    getFilteredRowModel: getGlobalFilteredRowModel(),
    state: {
      globalFilter,
    },
  });

  return (
    <>
      <input
        value={globalFilter}
        onChange={(e) => setGlobalFilter(e.target.value)}
        placeholder="Search all columns..."
      />
      <table>
        {/* ... table markup */}
      </table>
    </>
  );
}
```

### Custom Filter Function

```tsx
const customFilterFn: FilterFn<Person> = (row, columnId, filterValue) => {
  const value = row.getValue(columnId) as string;
  // Custom filter logic
  return value.toLowerCase().includes(filterValue.toLowerCase());
};

const columns: ColumnDef<Person>[] = [
  {
    accessorKey: 'name',
    header: 'Name',
    filterFn: customFilterFn,
  },
];
```

## Pagination

### Client-Side Pagination

```tsx
import { useState } from 'react';
import {
  useReactTable,
  getCoreRowModel,
  getPaginationRowModel,
  PaginationState,
} from '@tanstack/react-table';

function PaginatedTable() {
  const [pagination, setPagination] = useState<PaginationState>({
    pageIndex: 0,
    pageSize: 10,
  });

  const table = useReactTable({
    columns,
    data,
    getCoreRowModel: getCoreRowModel(),
    // Enable pagination
    enablePagination: true,
    // Handle pagination state changes
    onPaginationChange: setPagination,
    // Get paginated row model
    getPaginationRowModel: getPaginationRowModel(),
    state: {
      pagination,
    },
  });

  return (
    <>
      <table>
        {/* ... table markup */}
      </table>
      {/* Pagination controls */}
      <div>
        <button
          onClick={() => table.setPageIndex(0)}
          disabled={!table.getCanPreviousPage()}
        >
          {'<<'}
        </button>
        <button
          onClick={() => table.previousPage()}
          disabled={!table.getCanPreviousPage()}
        >
          {'<'}
        </button>
        <button
          onClick={() => table.nextPage()}
          disabled={!table.getCanNextPage()}
        >
          {'>'}
        </button>
        <button
          onClick={() => table.setPageIndex(table.getPageCount() - 1)}
          disabled={!table.getCanNextPage()}
        >
          {'>>'}
        </button>
        <span>
          Page{' '}
          <strong>
            {table.getState().pagination.pageIndex + 1} of{' '}
            {table.getPageCount()}
          </strong>
        </span>
        <select
          value={table.getState().pagination.pageSize}
          onChange={(e) => {
            table.setPageSize(Number(e.target.value));
          }}
        >
          {[10, 20, 30, 40, 50].map((pageSize) => (
            <option key={pageSize} value={pageSize}>
              Show {pageSize}
            </option>
          ))}
        </select>
      </div>
    </>
  );
}
```

### Server-Side Pagination

```tsx
import { useState } from 'react';
import {
  useReactTable,
  getCoreRowModel,
  PaginationState,
} from '@tanstack/react-table';

function ServerPaginatedTable() {
  const [pagination, setPagination] = useState<PaginationState>({
    pageIndex: 0,
    pageSize: 10,
  });

  // Fetch data based on pagination state
  const { data, totalCount } = useQuery({
    queryKey: ['users', pagination],
    queryFn: () => fetchUsers(pagination.pageIndex, pagination.pageSize),
  });

  const table = useReactTable({
    columns,
    data: data ?? [],
    getCoreRowModel: getCoreRowModel(),
    // Manual pagination - table won't paginate data itself
    manualPagination: true,
    // Total page count for server-side pagination
    pageCount: Math.ceil(totalCount / pagination.pageSize),
    onPaginationChange: setPagination,
    state: {
      pagination,
    },
  });

  return (
    <>
      <table>
        {/* ... table markup */}
      </table>
      {/* Pagination controls */}
    </>
  );
}
```

## Column Visibility

```tsx
import { useState } from 'react';
import {
  useReactTable,
  getCoreRowModel,
  VisibilityState,
} from '@tanstack/react-table';

function ColumnVisibilityTable() {
  const [columnVisibility, setColumnVisibility] = useState<VisibilityState>({});

  const table = useReactTable({
    columns,
    data,
    getCoreRowModel: getCoreRowModel(),
    // Handle column visibility changes
    onColumnVisibilityChange: setColumnVisibility,
    state: {
      columnVisibility,
    },
  });

  return (
    <>
      {/* Toggle column visibility */}
      <div>
        {table.getAllColumns()
          .filter((column) => column.getCanHide())
          .map((column) => (
            <label key={column.id}>
              <input
                type="checkbox"
                checked={column.getIsVisible()}
                onChange={(e) => column.toggleVisibility(e.target.checked)}
              />
              {column.id}
            </label>
          ))}
      </div>
      <table>
        {/* ... table markup */}
      </table>
    </>
  );
}
```

## Row Selection

```tsx
import { useState } from 'react';
import {
  useReactTable,
  getCoreRowModel,
  RowSelectionState,
} from '@tanstack/react-table';

function SelectableTable() {
  const [rowSelection, setRowSelection] = useState<RowSelectionState>({});

  const table = useReactTable({
    columns: [
      // Add selection column
      {
        id: 'select',
        header: ({ table }) => (
          <input
            type="checkbox"
            checked={table.getIsAllRowsSelected()}
            onChange={table.getToggleAllRowsSelectedHandler()}
          />
        ),
        cell: ({ row }) => (
          <input
            type="checkbox"
            checked={row.getIsSelected()}
            onChange={row.getToggleSelectedHandler()}
          />
        ),
      },
      ...otherColumns,
    ],
    data,
    getCoreRowModel: getCoreRowModel(),
    // Enable row selection
    enableRowSelection: true,
    // Handle row selection changes
    onRowSelectionChange: setRowSelection,
    state: {
      rowSelection,
    },
  });

  // Get selected rows
  const selectedRows = table.getFilteredSelectedRowModel().rows;
  const selectedRowIds = Object.keys(rowSelection);

  return (
    <>
      <div>
        Selected: {selectedRows.length} row(s)
      </div>
      <table>
        {/* ... table markup */}
      </table>
    </>
  );
}
```

## Column Resizing

```tsx
import { useState } from 'react';
import {
  useReactTable,
  getCoreRowModel,
  ColumnSizingState,
} from '@tanstack/react-table';

function ResizableTable() {
  const [columnSizing, setColumnSizing] = useState<ColumnSizingState>({});

  const table = useReactTable({
    columns,
    data,
    getCoreRowModel: getCoreRowModel(),
    // Enable column resizing
    enableColumnResizing: true,
    // Column resizing mode
    columnResizeMode: 'onChange', // or 'onEnd'
    // Handle column sizing changes
    onColumnSizingChange: setColumnSizing,
    state: {
      columnSizing,
    },
  });

  return (
    <table style={{ width: table.getCenterTotalSize() }}>
      <thead>
        {table.getHeaderGroups().map(headerGroup => (
          <tr key={headerGroup.id}>
            {headerGroup.headers.map(header => (
              <th
                key={header.id}
                style={{ width: header.getSize() }}
              >
                {flexRender(header.column.columnDef.header, header.getContext())}
                {/* Resize handle */}
                <div
                  onMouseDown={header.getResizeHandler()}
                  onTouchStart={header.getResizeHandler()}
                  className={`resizer ${header.column.getIsResizing() ? 'isResizing' : ''}`}
                />
              </th>
            ))}
          </tr>
        ))}
      </thead>
      {/* ... rest of table */}
    </table>
  );
}
```

## Column Ordering (Drag & Drop)

```tsx
import { useState } from 'react';
import {
  useReactTable,
  getCoreRowModel,
  ColumnOrderState,
} from '@tanstack/react-table';

function ReorderableTable() {
  const [columnOrder, setColumnOrder] = useState<ColumnOrderState>([]);

  const table = useReactTable({
    columns,
    data,
    getCoreRowModel: getCoreRowModel(),
    // Handle column order changes
    onColumnOrderChange: setColumnOrder,
    state: {
      columnOrder,
    },
  });

  // Reorder columns programmatically
  const moveColumn = (fromIndex: number, toIndex: number) => {
    const newOrder = [...columnOrder];
    const [removed] = newOrder.splice(fromIndex, 1);
    newOrder.splice(toIndex, 0, removed);
    setColumnOrder(newOrder);
  };

  return (
    <table>
      {/* ... table markup */}
    </table>
  );
}
```

## Expanding Rows (Sub-rows)

```tsx
import { useState } from 'react';
import {
  useReactTable,
  getCoreRowModel,
  getExpandedRowModel,
  ExpandedState,
} from '@tanstack/react-table';

function ExpandableTable() {
  const [expanded, setExpanded] = useState<ExpandedState>({});

  const table = useReactTable({
    columns: [
      {
        id: 'expander',
        header: () => null,
        cell: ({ row }) => (
          <button
            onClick={() => row.toggleExpanded()}
            disabled={!row.getCanExpand()}
          >
            {row.getIsExpanded() ? '👇' : '👉'}
          </button>
        ),
      },
      ...otherColumns,
    ],
    data,
    getCoreRowModel: getCoreRowModel(),
    // Enable row expansion
    enableExpanding: true,
    // Handle expansion state changes
    onExpandedChange: setExpanded,
    // Get expanded row model
    getExpandedRowModel: getExpandedRowModel(),
    getSubRows: (row) => row.subRows, // Define how to get sub-rows
    state: {
      expanded,
    },
  });

  return (
    <table>
      <tbody>
        {table.getRowModel().rows.map(row => (
          <>
            <tr key={row.id}>
              {row.getVisibleCells().map(cell => (
                <td key={cell.id}>
                  {flexRender(cell.column.columnDef.cell, cell.getContext())}
                </td>
              ))}
            </tr>
            {/* Render sub-rows when expanded */}
            {row.getIsExpanded() && row.subRows?.map(subRow => (
              <tr key={subRow.id}>
                {subRow.getVisibleCells().map(cell => (
                  <td key={cell.id} style={{ paddingLeft: '2rem' }}>
                    {flexRender(cell.column.columnDef.cell, cell.getContext())}
                  </td>
                ))}
              </tr>
            ))}
          </>
        ))}
      </tbody>
    </table>
  );
}
```

## Complete Example: Full-Featured Table

```tsx
import { useState } from 'react';
import {
  useReactTable,
  getCoreRowModel,
  getSortedRowModel,
  getFilteredRowModel,
  getPaginationRowModel,
  flexRender,
  ColumnDef,
  SortingState,
  ColumnFiltersState,
  PaginationState,
} from '@tanstack/react-table';

type Person = {
  id: number;
  name: string;
  age: number;
  email: string;
  status: 'active' | 'inactive';
};

const columns: ColumnDef<Person>[] = [
  {
    accessorKey: 'name',
    header: 'Name',
    enableSorting: true,
    enableColumnFilter: true,
  },
  {
    accessorKey: 'age',
    header: 'Age',
    enableSorting: true,
    filterFn: 'inNumberRange',
  },
  {
    accessorKey: 'email',
    header: 'Email',
  },
  {
    accessorKey: 'status',
    header: 'Status',
    filterFn: 'equalsString',
  },
];

function FullFeaturedTable({ data }: { data: Person[] }) {
  const [sorting, setSorting] = useState<SortingState>([]);
  const [columnFilters, setColumnFilters] = useState<ColumnFiltersState>([]);
  const [pagination, setPagination] = useState<PaginationState>({
    pageIndex: 0,
    pageSize: 10,
  });

  const table = useReactTable({
    columns,
    data,
    getCoreRowModel: getCoreRowModel(),
    getSortedRowModel: getSortedRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
    onSortingChange: setSorting,
    onColumnFiltersChange: setColumnFilters,
    onPaginationChange: setPagination,
    state: {
      sorting,
      columnFilters,
      pagination,
    },
  });

  return (
    <div>
      {/* Global filter */}
      <input
        value={(table.getColumn('name')?.getFilterValue() as string) ?? ''}
        onChange={(e) => table.getColumn('name')?.setFilterValue(e.target.value)}
        placeholder="Filter by name..."
      />

      {/* Table */}
      <table>
        <thead>
          {table.getHeaderGroups().map(headerGroup => (
            <tr key={headerGroup.id}>
              {headerGroup.headers.map(header => (
                <th
                  key={header.id}
                  onClick={header.column.getToggleSortingHandler()}
                  style={{ cursor: header.column.getCanSort() ? 'pointer' : 'default' }}
                >
                  {flexRender(header.column.columnDef.header, header.getContext())}
                  {{
                    asc: ' 🔼',
                    desc: ' 🔽',
                  }[header.column.getIsSorted() as string] ?? null}
                </th>
              ))}
            </tr>
          ))}
        </thead>
        <tbody>
          {table.getRowModel().rows.map(row => (
            <tr key={row.id}>
              {row.getVisibleCells().map(cell => (
                <td key={cell.id}>
                  {flexRender(cell.column.columnDef.cell, cell.getContext())}
                </td>
              ))}
            </tr>
          ))}
        </tbody>
      </table>

      {/* Pagination */}
      <div>
        <button
          onClick={() => table.setPageIndex(0)}
          disabled={!table.getCanPreviousPage()}
        >
          {'<<'}
        </button>
        <button
          onClick={() => table.previousPage()}
          disabled={!table.getCanPreviousPage()}
        >
          {'<'}
        </button>
        <span>
          Page {table.getState().pagination.pageIndex + 1} of{' '}
          {table.getPageCount()}
        </span>
        <button
          onClick={() => table.nextPage()}
          disabled={!table.getCanNextPage()}
        >
          {'>'}
        </button>
        <button
          onClick={() => table.setPageIndex(table.getPageCount() - 1)}
          disabled={!table.getCanNextPage()}
        >
          {'>>'}
        </button>
      </div>
    </div>
  );
}
```

## Common Table Instance APIs

```tsx
const table = useReactTable({ columns, data, ... });

// State access
table.getState()                    // Get all state
table.getState().sorting            // Get sorting state
table.getState().columnFilters      // Get filter state
table.getState().pagination         // Get pagination state

// Row models
table.getRowModel()                 // Get current row model (after all transformations)
table.getCoreRowModel()             // Get core row model
table.getFilteredRowModel()         // Get filtered row model
table.getSortedRowModel()           // Get sorted row model
table.getPaginationRowModel()       // Get paginated row model

// Column APIs
table.getColumn('name')             // Get column by id
table.getAllColumns()               // Get all columns
table.getVisibleColumns()           // Get visible columns
table.getHeaderGroups()             // Get header groups

// Sorting APIs
table.setSorting([{ id: 'name', desc: true }])
table.resetSorting()

// Filtering APIs
table.setColumnFilters([{ id: 'name', value: 'John' }])
table.resetColumnFilters()
table.getColumn('name')?.setFilterValue('John')
table.getColumn('name')?.getFilterValue()

// Pagination APIs
table.nextPage()
table.previousPage()
table.setPageIndex(2)
table.setPageSize(20)
table.getCanNextPage()
table.getCanPreviousPage()
table.getPageCount()

// Row selection APIs
table.toggleAllRowsSelected()
table.getIsAllRowsSelected()
table.getSelectedRowModel()
```

## Best Practices

1. **Use TypeScript** - Define types for your data and columns
2. **Memoize columns** - Use `useMemo` for column definitions if they depend on props
3. **Memoize data** - Use `useMemo` for data transformations
4. **Server-side for large datasets** - Use `manualPagination`, `manualSorting`, `manualFiltering` for large datasets
5. **Virtualization** - Consider `@tanstack/react-virtual` for very large tables
6. **Accessibility** - Add proper ARIA labels and keyboard navigation
7. **Performance** - Only enable features you need; each row model adds overhead



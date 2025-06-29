# Andrea Table

[![npm version](https://badge.fury.io/js/andrea-table.svg)](https://badge.fury.io/js/andrea-table)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/derekzyl/andrea-table.svg)](https://github.com/derekzyl/andrea-table)

> A powerful, feature-rich React table component with advanced filtering, sorting, pagination, and CRUD operations.

## ✨ Features

- 🚀 **Remote Data Fetching** - Seamlessly fetch data from APIs
- 🔄 **Real-time Sorting** - Multi-column sorting with visual indicators
- 🔍 **Advanced Filtering** - Text, boolean, and date-based filters
- 📄 **Pagination** - Efficient data pagination with customizable page sizes
- ⚡ **CRUD Operations** - Built-in Create, Read, Update, Delete functionality
- 🎨 **Fully Customizable** - Colors, buttons, columns, and behavior
- 📱 **Responsive Design** - Works seamlessly across all devices
- 🔧 **TypeScript Support** - Full type safety and IntelliSense

## 🚀 Quick Start

### Installation

```bash
npm install andrea-table
# or
yarn add andrea-table
# or
pnpm add andrea-table
```

### Basic Usage

```tsx
import { NewTable, TableDataT } from 'andrea-table';

const tableConfig: TableDataT<User> = {
  tableName: "Users",
  baseUrl: "https://api.example.com",
  subUrl: "/users",
  heading: [
    { key: "id", name: "ID", canSort: true, canFilter: false },
    { key: "name", name: "Name", canSort: true, canFilter: true },
    { key: "email", name: "Email", canSort: true, canFilter: true },
  ],
  fn: {
    fetchFn: async ({ url, baseUrl }) => {
      const response = await fetch(baseUrl + url);
      return response.json();
    }
  },
  show: { pagination: true, search: true, filters: true },
  query: { pageName: "page", limitName: "limit" },
  crud: {}
};

function App() {
  return <NewTable data={tableConfig} />;
}
```

## 📖 Documentation

### Table Configuration

#### `TableDataT<T>` Interface

| Property | Type | Description | Required |
|----------|------|-------------|----------|
| `tableName` | `string` | Display name for the table | ✅ |
| `baseUrl` | `string` | Base API URL | ✅ |
| `subUrl` | `string` | Endpoint path | ✅ |
| `heading` | `HeadingT<T>[]` | Column definitions | ✅ |
| `fn.fetchFn` | `Function` | Data fetching function | ✅ |
| `show` | `ShowOptions` | UI element visibility | ✅ |
| `query` | `QueryConfig` | API query parameters | ✅ |
| `crud` | `CrudConfig` | CRUD operation settings | ✅ |
| `column` | `ColumnT<T>[]` | Custom column components | ❌ |
| `color` | `ColorTheme` | Color customization | ❌ |
| `refresh` | `RefreshConfig` | Auto-refresh settings | ❌ |
| `buttonName` | `ButtonNames` | Custom button labels | ❌ |

#### `HeadingT<T>` - Column Definition

```typescript
type HeadingT<T> = {
  key: keyof T | string;           // Data property key
  name: string;                    // Display name
  isHeader?: boolean;              // Show in header (default: true)
  canSort?: boolean;               // Enable sorting (default: false)
  canFilter?: boolean;             // Enable filtering (default: false)
  canCopy?: boolean;               // Enable copy functionality (default: false)
  filters?: string[];              // Custom filter options
};
```

### Color Customization

```typescript
const colorTheme = {
  primary: "#3b82f6",           // Primary buttons and accents
  secondary: "#6b7280",         // Secondary elements
  tertiary: "#10b981",          // Success/tertiary actions
  background: "#ffffff",        // Main background
  cellBackground: "#f9fafb",    // Table cell background
  filterBackground: "#f3f4f6",  // Filter section background
  exportBackground: "#e5e7eb"   // Export button background
};
```

### Custom Column Components

Create custom renderers for complex data:

```tsx
const AddressColumn: React.FC<ColumnElementT<User>> = ({ columnData }) => (
  <div className="space-y-1">
    <div className="font-medium">{columnData.address.street}</div>
    <div className="text-sm text-gray-500">
      {columnData.address.city}, {columnData.address.state}
    </div>
  </div>
);

const ActionColumn: React.FC<ColumnElementT<User>> = ({ columnData, onDeleteSuccess }) => (
  <div className="flex gap-2">
    <button onClick={() => editUser(columnData.id)} className="btn-primary">
      Edit
    </button>
    <button onClick={() => deleteUser(columnData.id)} className="btn-danger">
      Delete
    </button>
  </div>
);

const extraColumns: ColumnT<User>[] = [
  { _address: <AddressColumn columnData={""} crud={{}} /> },
  { action: <ActionColumn columnData={""} crud={{}} /> }
];
```

### Advanced Features

#### Auto-refresh
```typescript
refresh: { 
  status: true, 
  intervalInSec: 30 // Refresh every 30 seconds
}
```

#### CRUD Operations
```typescript
crud: {
  add: true,      // Show add button
  edit: true,     // Enable edit functionality
  delete: true,   // Enable delete functionality
  view: true,     // Enable view functionality
  export: true    // Enable export functionality
}
```

#### UI Control
```typescript
show: {
  filters: true,        // Show filter controls
  pagination: true,     // Show pagination
  search: true,         // Show global search
  select: true,         // Show row selection
  sort: true,           // Enable column sorting
  table: true,          // Show the table
  exports: true,        // Show export options
  addButton: true,      // Show add new button
  checkBox: true,       // Show checkboxes
  customButton: true,   // Show custom button
  seeMore: true,        // Show see more option
  tableName: true       // Show table name
}
```

## 🎯 Examples

### Complete User Management Table

```tsx
import { NewTable, TableDataT, HeadingT, ColumnT } from 'andrea-table';

interface User {
  id: number;
  firstName: string;
  lastName: string;
  email: string;
  address: {
    street: string;
    city: string;
    state: string;
    postalCode: string;
    country: string;
  };
}

const userTableConfig: TableDataT<User> = {
  tableName: "User Management",
  baseUrl: "https://dummyjson.com",
  subUrl: "/users",
  heading: [
    { key: "id", name: "ID", canSort: true, canCopy: true },
    { key: "firstName", name: "First Name", canSort: true, canFilter: true },
    { key: "lastName", name: "Last Name", canSort: true, canFilter: true },
    { key: "email", name: "Email", canSort: true, canFilter: true, canCopy: true },
    { key: "_address", name: "Address", canFilter: true },
    { key: "action", name: "Actions", canSort: false }
  ],
  fn: {
    fetchFn: async ({ url, baseUrl }) => {
      const response = await fetch(baseUrl + url);
      const data = await response.json();
      return data.users;
    },
    addFn: () => console.log("Add new user"),
    customFn: () => console.log("Custom action")
  },
  show: {
    pagination: true,
    search: true,
    filters: true,
    addButton: true,
    customButton: true,
    exports: true
  },
  query: { pageName: "skip", limitName: "limit" },
  color: {
    primary: "#3b82f6",
    secondary: "#6b7280",
    tertiary: "#10b981"
  },
  crud: {
    add: true,
    edit: true,
    delete: true,
    view: true,
    export: true
  }
};

export function UserTable() {
  return (
    <div className="p-6">
      <NewTable data={userTableConfig} />
    </div>
  );
}
```

## 🔧 API Reference

### Data Fetching Function

Your fetch function should follow this signature:

```typescript
type FetchFunction = ({ url, baseUrl }: { 
  url: string; 
  baseUrl: string; 
}) => Promise<any[]>;
```

The function receives the constructed URL and should return an array of data objects.

### Event Handlers

```typescript
// Custom column component props
interface ColumnElementT<T> {
  columnData: T;
  crud: CrudConfig;
  onDeleteSuccess?: () => void;
  onEditSuccess?: () => void;
  onAddSuccess?: () => void;
}
```

## 🎨 Styling

Andrea Table uses Tailwind CSS classes by default. You can customize the appearance by:

1. **Using the color prop** - Override default colors
2. **Custom CSS classes** - Add your own styling
3. **Tailwind utilities** - Use Tailwind classes in custom components

## 🤝 Contributing

We welcome contributions! Please see our [contributing guidelines](https://github.com/derekzyl/andrea-table/blob/main/CONTRIBUTING.md) for details.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Support

If you find this project helpful, please consider:

- ⭐ [Starring the repository](https://github.com/derekzyl/andrea-table)
- 🐛 [Reporting bugs](https://github.com/derekzyl/andrea-table/issues)
- 💡 [Suggesting new features](https://github.com/derekzyl/andrea-table/issues)
- ☕ [Buying me a coffee](https://buymeacoffee.com/cybergenii)

### Cryptocurrency Donations
- **Solana**: `cSntSgCMytF1wjdGpa2tYt7gpgAxCNNM4QGVN9xjJSo`
- **Bitcoin**: `18Zne6NrvrvYYM83hKeCqGoBVWeyhfpym1`

---

<div align="center">
  Made with ❤️ by <a href="https://github.com/derekzyl">Derek</a>
</div>

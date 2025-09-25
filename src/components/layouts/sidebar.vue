<template>
  <a-layout-sider
    v-model:collapsed="collapsed"
    collapsible
    class="overflow-y-auto whitespace-nowrap modern-sidebar"
    :width="230"
    :collapsed-width="80"
  >
    <!-- Header Section -->
    <div class="sidebar-header">
      <div class="logo-container">
        <div class="logo-wrapper">
          <img
            v-if="!collapsed"
            src="/src/assets/images/fav.png"
            alt="Logo"
            class="logo-image"
          />
          <div v-else class="logo-collapsed">
            <Icon icon="mdi:medical-bag" class="w-8 h-8 text-white" />
          </div>
        </div>

        <div v-if="!collapsed" class="brand-text">
          <h2 class="brand-title">
            Ousad<span class="brand-subtitle">Bazar</span>
          </h2>
          <span class="admin-label">Admin Panel</span>
        </div>
      </div>
    </div>

    <!-- Navigation Menu -->
    <div class="menu-container">
      <a-menu
        v-model:openKeys="state.openKeys"
        v-model:selectedKeys="state.selectedKeys"
        mode="inline"
        theme="dark"
        :inline-collapsed="collapsed"
        :items="items"
        class="custom-menu"
      ></a-menu>
    </div>

    <!-- Footer Section (when expanded) -->
    <!-- <div v-if="!collapsed" class="sidebar-footer">
      <div class="footer-content">
        <div class="status-indicator">
          <Icon
            icon="mdi:circle"
            class="w-2 h-2 text-green-400 animate-pulse"
          />
          <span class="status-text">System Online</span>
        </div>
        <div class="version-info">
          <span class="version-text">v2.1.0</span>
        </div>
      </div>
    </div> -->
  </a-layout-sider>
</template>

<script setup>
import { ref, watch, reactive, h } from "vue";
import { Icon } from "@iconify/vue";
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();

const collapsed = ref(false);
const state = reactive({
  openKeys: [],
  selectedKeys: [route.path],
});

const items = reactive([
  {
    key: "/home",
    label: "Dashboard",
    icon: () => h(Icon, { icon: "mdi:view-dashboard", class: "menu-icon" }),
    onClick: () => router.push("/home"),
  },
  {
    key: "pending-list",
    label: "Order Management",
    icon: () => h(Icon, { icon: "mdi:clipboard-list", class: "menu-icon" }),
    children: [
      {
        key: "/pending-list",
        label: "Pending Orders",
        icon: () => h(Icon, { icon: "mdi:clock-alert", class: "submenu-icon" }),
        onClick: () => router.push("/pending-list"),
      },
      {
        key: "/all-order-list",
        label: "All Orders",
        icon: () =>
          h(Icon, { icon: "mdi:format-list-bulleted", class: "submenu-icon" }),
        onClick: () => router.push("/all-order-list"),
      },
    ],
  },
  {
    key: "products",
    label: "Product Management",
    icon: () => h(Icon, { icon: "mdi:package-variant", class: "menu-icon" }),
    children: [
      {
        key: "/products",
        label: "All Products",
        icon: () => h(Icon, { icon: "mdi:package", class: "submenu-icon" }),
        onClick: () => router.push("/products"),
      },
      {
        key: "/categories",
        label: "Categories",
        icon: () =>
          h(Icon, { icon: "mdi:tag-multiple", class: "submenu-icon" }),
        onClick: () => router.push("/categories"),
      },
    ],
  },
  {
    key: "users",
    label: "User Management",
    icon: () => h(Icon, { icon: "mdi:account-group", class: "menu-icon" }),
    children: [
      {
        key: "/users",
        label: "Customers",
        icon: () =>
          h(Icon, { icon: "mdi:account-multiple", class: "submenu-icon" }),
        onClick: () => router.push("/users"),
      },
      {
        key: "/delivery-men",
        label: "Delivery Staff",
        icon: () =>
          h(Icon, { icon: "mdi:truck-delivery", class: "submenu-icon" }),
        onClick: () => router.push("/delivery-men"),
      },
    ],
  },
  {
    key: "reports",
    label: "Reports & Analytics",
    icon: () => h(Icon, { icon: "mdi:chart-bar", class: "menu-icon" }),
    children: [
      {
        key: "/sales-report",
        label: "Sales Report",
        icon: () => h(Icon, { icon: "mdi:chart-line", class: "submenu-icon" }),
        onClick: () => router.push("/sales-report"),
      },
      {
        key: "/inventory-report",
        label: "Inventory Report",
        icon: () => h(Icon, { icon: "mdi:warehouse", class: "submenu-icon" }),
        onClick: () => router.push("/inventory-report"),
      },
    ],
  },
  {
    key: "settings",
    label: "Settings",
    icon: () => h(Icon, { icon: "mdi:cog", class: "menu-icon" }),
    onClick: () => router.push("/settings"),
  },
]);

watch(
  () => route.path,
  (newPath) => {
    const parentKey = items.find((item) =>
      item.children?.some((child) => newPath.startsWith(child.key))
    )?.key;

    state.openKeys = parentKey ? [parentKey] : [];
    state.selectedKeys = [
      items
        .flatMap((item) => (item.children ? item.children : item))
        .find((child) => newPath.startsWith(child.key))?.key || newPath,
    ];
  },
  { immediate: true }
);
</script>

<style scoped>
/* Modern Sidebar Styles */
.modern-sidebar {
  background: linear-gradient(180deg, #1e293b 0%, #0f172a 100%) !important;
  border-right: 1px solid #334155 !important;
  box-shadow: 4px 0 12px rgba(0, 0, 0, 0.15) !important;
}

/* Header Section */
.sidebar-header {
  padding: 1.5rem 1rem;
  border-bottom: 1px solid #334155;
  background: rgba(59, 130, 246, 0.1);
}

.logo-container {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.logo-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo-image {
  width: 3rem;
  height: 3rem;
  border-radius: 0.75rem;
  border: 2px solid #3b82f6;
  box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
}

.logo-collapsed {
  width: 3rem;
  height: 3rem;
  background: #3b82f6;
  border-radius: 0.75rem;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
}

.brand-text {
  display: flex;
  flex-direction: column;
}

.brand-title {
  color: white;
  font-size: 1.25rem;
  font-weight: 700;
  margin: 0;
  line-height: 1.2;
}

.brand-subtitle {
  color: #3b82f6;
  font-weight: 600;
}

.admin-label {
  color: #94a3b8;
  font-size: 0.875rem;
  font-weight: 500;
  margin-top: 0.25rem;
}

/* Menu Container */
.menu-container {
  padding: 1rem 0.5rem;
  flex: 1;
}

/* Footer Section */
.sidebar-footer {
  padding: 1rem;
  border-top: 1px solid #334155;
  background: rgba(0, 0, 0, 0.2);
}

.footer-content {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.status-indicator {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.status-text {
  color: #94a3b8;
  font-size: 0.75rem;
  font-weight: 500;
}

.version-info {
  text-align: center;
}

.version-text {
  color: #64748b;
  font-size: 0.75rem;
  font-weight: 400;
}

/* Menu Icon Styles */
:global(.menu-icon) {
  width: 1.25rem;
  height: 1.25rem;
  color: #94a3b8;
}

:global(.submenu-icon) {
  width: 1rem;
  height: 1rem;
  color: #94a3b8;
}

/* Custom Menu Styles */
:global(.custom-menu) {
  background: transparent !important;
  border: none !important;
}

:global(.custom-menu .ant-menu-item) {
  margin: 0.25rem 0 !important;
  border-radius: 0.5rem !important;
  height: auto !important;
  line-height: 1.5 !important;
  padding: 0.75rem 1rem !important;
  transition: all 0.2s ease-in-out !important;
}

:global(.custom-menu .ant-menu-submenu-title) {
  margin: 0.25rem 0 !important;
  border-radius: 0.5rem !important;
  height: auto !important;
  line-height: 1.5 !important;
  padding: 0.75rem 1rem !important;
  transition: all 0.2s ease-in-out !important;
}

:global(.custom-menu .ant-menu-item:hover),
:global(.custom-menu .ant-menu-submenu-title:hover) {
  background: rgba(59, 130, 246, 0.2) !important;
  color: white !important;
  transform: translateX(4px);
}

:global(.custom-menu .ant-menu-item-selected) {
  background: linear-gradient(135deg, #3b82f6 0%, #1d4ed8 100%) !important;
  color: white !important;
  box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3) !important;
  transform: translateX(4px);
}

:global(.custom-menu .ant-menu-item-selected .menu-icon),
:global(.custom-menu .ant-menu-item-selected .submenu-icon) {
  color: white !important;
}

:global(.custom-menu .ant-menu-submenu-open > .ant-menu-submenu-title) {
  background: rgba(59, 130, 246, 0.15) !important;
  color: #3b82f6 !important;
}

:global(.custom-menu .ant-menu-sub) {
  background: rgba(15, 23, 42, 0.8) !important;
  border-radius: 0.5rem !important;
  margin: 0.5rem 0 !important;
  padding: 0.5rem !important;
}

:global(.custom-menu .ant-menu-item-group-title) {
  color: #64748b !important;
  font-size: 0.75rem !important;
  font-weight: 600 !important;
  text-transform: uppercase !important;
  letter-spacing: 0.05em !important;
  padding: 0.5rem 1rem !important;
}

/* Trigger Button Styling */
:global(.ant-layout-sider-trigger) {
  background: #1e293b !important;
  border-top: 1px solid #334155 !important;
  color: #94a3b8 !important;
  transition: all 0.2s ease-in-out !important;
}

:global(.ant-layout-sider-trigger:hover) {
  background: #3b82f6 !important;
  color: white !important;
}

/* Scrollbar Styling */
.modern-sidebar::-webkit-scrollbar {
  width: 6px;
}

.modern-sidebar::-webkit-scrollbar-track {
  background: #1e293b;
}

.modern-sidebar::-webkit-scrollbar-thumb {
  background: #475569;
  border-radius: 3px;
}

.modern-sidebar::-webkit-scrollbar-thumb:hover {
  background: #64748b;
}

/* Animation for menu items */
:global(.ant-menu-item),
:global(.ant-menu-submenu-title) {
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1) !important;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .sidebar-header {
    padding: 1rem 0.75rem;
  }

  .logo-image,
  .logo-collapsed {
    width: 2.5rem;
    height: 2.5rem;
  }

  .brand-title {
    font-size: 1.125rem;
  }
}
</style>
